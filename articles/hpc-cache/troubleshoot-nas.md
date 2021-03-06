---
title: Устранение неполадок с целевыми объектами хранилища NFS в кэше Azure HPC
description: Советы по предотвращению и устранению ошибок конфигурации и другим проблемам, которые могут привести к сбою при создании целевого объекта хранилища NFS
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: rohogue
ms.openlocfilehash: 72b6b0b78da23fd0891c0571c9137fefbfb0b077
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82186623"
---
# <a name="troubleshoot-nas-configuration-and-nfs-storage-target-issues"></a>Устранение неполадок конфигурации NAS и целевого объекта хранилища NFS

В этой статье приводятся решения для некоторых распространенных ошибок конфигурации и других проблем, которые могут помешать кэшу Azure HPC добавить систему хранения NFS в качестве целевого объекта хранилища.

Эта статья содержит сведения о том, как проверить порты и как включить корневой доступ к системе NAS. Здесь также содержатся подробные сведения о менее распространенных проблемах, которые могут привести к сбою при создании целевого объекта хранилища NFS.

> [!TIP]
> Прежде чем использовать это пошаговое руководством, прочитайте [Предварительные требования для целевых объектов хранилища NFS](hpc-cache-prereqs.md#nfs-storage-requirements).

Если решение проблемы не включено в эту проблему, отправьте запрос в службу [поддержки](hpc-cache-support-ticket.md) , чтобы служба и поддержка Майкрософт могли работать с вами для изучения и устранения проблемы.

## <a name="check-port-settings"></a>Проверка параметров порта

Для кэша HPC Azure требуется доступ на чтение и запись к нескольким портам UDP/TCP в серверной системе хранения данных NAS. Убедитесь, что эти порты доступны в системе NAS, а трафик разрешен для этих портов через брандмауэры между системой хранения и подсетью кэша. Для проверки этой конфигурации может потребоваться работа с брандмауэром и администраторами сети для центра обработки данных.

Порты отличаются для систем хранения от разных поставщиков, поэтому проверьте требования к системе при настройке целевого объекта хранилища.

Как правило, для кэша требуется доступ к следующим портам:

| Протокол | Порт  | Служба  |
|----------|-------|----------|
| TCP/UDP  | 111   | rpcbind  |
| TCP/UDP  | 2049  | NFS      |
| TCP/UDP  | 4045  | нлоккмгр |
| TCP/UDP  | 4046  | подключено   |
| TCP/UDP  | 4047  | status   |

Чтобы узнать, какие порты необходимы для вашей системы, используйте следующую ``rpcinfo`` команду. Эта команда перечисляет порты и форматирует соответствующие результаты в таблице. (Используйте IP-адрес системы вместо *<storage_IP>* .)

Эту команду можно выполнить из любого клиента Linux с установленной инфраструктурой NFS. Если вы используете клиент в подсети кластера, он также может помочь проверить подключение между подсетью и системой хранения данных.

```bash
rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
```

Убедитесь, что все порты, возвращенные ``rpcinfo`` запросом, разрешают неограниченный трафик из подсети кэша HPC Azure.

Проверьте эти параметры как на самом NAS, так и во всех брандмауэрах между системой хранения и подсетью кэша.

## <a name="check-root-access"></a>Проверка доступа root

Для создания целевого объекта хранилища кэш Azure HPC должен иметь доступ к экспортам системы хранения. В частности, он подключает экспорты в качестве пользователя с ИДЕНТИФИКАТОРом 0.

Различные системы хранения используют различные методы для включения этого доступа:

* Серверы Linux обычно добавляются ``no_root_squash`` к экспортированному пути ``/etc/exports``в.
* Системы NetApp и EMC обычно контролируют доступ с помощью правил экспорта, привязанных к конкретным IP-адресам или сетям.

При использовании правил экспорта Помните, что кэш может использовать несколько разных IP-адресов из подсети кэша. Разрешение доступа из полного диапазона возможных IP-адресов подсети.

> [!NOTE]
> По умолчанию кэш Azure HPC скуашес корневой доступ. Дополнительные сведения см. в статье [Настройка дополнительных параметров кэша](configuration.md#configure-root-squash) .

Обратитесь к поставщику хранилища NAS, чтобы обеспечить правильный уровень доступа к кэшу.

### <a name="allow-root-access-on-directory-paths"></a>Разрешить корневой доступ по путям к каталогам
<!-- linked in prereqs article -->

Для систем NAS, экспортирующих иерархические каталоги, кэш HPC Azure должен иметь корневой доступ к каждому уровню экспорта.

Например, в системе могут отображаться три экспорта, подобные следующим:

* ``/ifs``
* ``/ifs/accounting``
* ``/ifs/accounting/payroll``

Экспорт ``/ifs/accounting/payroll`` является дочерним элементом ``/ifs/accounting``, а ``/ifs/accounting`` сам по себе является дочерним элементом ``/ifs``.

Если вы добавляете ``payroll`` экспорт в качестве целевого объекта хранилища кэша HPC, кэш фактически подключается ``/ifs/`` и обращается к каталогу зарплаты из него. Поэтому для доступа к ``/ifs`` ``/ifs/accounting/payroll`` экспорту кэш Azure HPC должен иметь права доступа root.

Это требование связано с тем, как кэш файлов индексируется и позволяет избежать конфликтов файлов, используя дескрипторы файлов, предоставляемые системой хранения.

Система NAS с иерархическими экспортами может предоставить разные дескрипторы файлов для одного и того же файла, если файл извлекается из разных экспортов. Например, клиент может подключить ``/ifs/accounting`` файл ``payroll/2011.txt``и получить к нему доступ. Другой клиент подключает ``/ifs/accounting/payroll`` и обращается к файлу ``2011.txt``. В зависимости от того, как система хранения назначает дескрипторы файлов, эти два клиента могут получить один и тот же файл с разными дескрипторами ``<mount2>/payroll/2011.txt`` файлов (один ``<mount3>/2011.txt``для и один для).

Внутренняя система хранения хранит внутренние псевдонимы для дескрипторов файлов, но кэш Azure HPC не может определить, какие дескрипторы файлов в его индексе ссылаются на один и тот же элемент. Поэтому возможно, что кэш может иметь различные операции записи, кэшированные для одного и того же файла, и применить изменения неверно, так как не знает, что они являются одним и тем же файлом.

Чтобы избежать такого возможного конфликта файлов в нескольких экспортах, кэш Azure HPC автоматически подключает неглубокий доступный экспорт в пути (``/ifs`` в примере) и использует файловый обработчик, заданный для этого экспорта. Если несколько экспортов используют один и тот же базовый путь, то кэш Azure HPC должен иметь корневой доступ к этому пути.

## <a name="enable-export-listing"></a>Включить листинг экспорта
<!-- link in prereqs article -->

Если кэш Azure HPC запрашивает его, NAS должен вывести список экспортируемых компонентов.

В большинстве систем хранения NFS это можно проверить, отправив следующий запрос из клиента Linux:``showmount -e <storage IP address>``

Используйте клиент Linux из той же виртуальной сети, что и ваш кэш, если это возможно.

Если эта команда не выводит список экспортируемых компонентов, кэш будет иметь проблемы с подключением к системе хранения. Обратитесь к поставщику NAS, чтобы включить вывод списка экспорта.

## <a name="adjust-vpn-packet-size-restrictions"></a>Настройка ограничений по размеру пакетов VPN
<!-- link in prereqs article and configuration article -->

При наличии VPN между кэшем и устройством NAS VPN может блокировать полный 1500-байтовый пакет Ethernet. Эта проблема может возникнуть, если большие операции обмена данными между NAS и экземпляром кэша HPC Azure не завершены, но небольшие обновления работают должным образом.

Нет простого способа определить, имеет ли ваша система эту проблему, если вы не знакомы с конфигурацией VPN. Вот несколько способов, которые могут помочь при проверке этой проблемы.

* Используйте средства прослушивания пакетов на обеих сторонах VPN, чтобы определить, какие пакеты были успешно переданы.
* Если VPN допускает команды ping, можно протестировать отправку пакета с полным размером.

  Выполните команду ping через VPN для NAS с помощью этих параметров. (Используйте IP-адрес системы хранения вместо значения *<storage_IP>* .)

   ```bash
   ping -M do -s 1472 -c 1 <storage_IP>
   ```

  Ниже приведены параметры команды.

  * ``-M do``— Не фрагментировать
  * ``-c 1``-Отправить только один пакет
  * ``-s 1472``— Установите размер полезных данных равным 1472 байт. Это максимальный размер полезных данных для 1500-байтового пакета после учета накладных расходов Ethernet.

  Успешный ответ выглядит следующим образом.

  ```bash
  PING 10.54.54.11 (10.54.54.11) 1472(1500) bytes of data.
  1480 bytes from 10.54.54.11: icmp_seq=1 ttl=64 time=2.06 ms
  ```

  Если проверка связи завершается сбоем с 1472 байтами, вероятно, существует проблема с размером пакета.

Чтобы устранить эту проблему, может потребоваться настроить фиксацию MSS в VPN, чтобы удаленная система правильно определяла максимальный размер кадра. Дополнительные сведения см. в [документации по параметрам IPsec и IKE VPN-шлюза](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) .

В некоторых случаях может помочь изменение параметра MTU для кэша HPC Azure на 1400. Однако если ограничить MTU в кэше, необходимо также ограничить параметры MTU для клиентов и серверных систем хранения, взаимодействующих с кэшем. Дополнительные сведения см. в статье [Настройка дополнительных параметров кэша Azure HPC](configuration.md#adjust-mtu-value) .

## <a name="check-for-acl-security-style"></a>Проверить стиль безопасности ACL

Некоторые системы NAS используют гибридный стиль безопасности, объединяющий списки управления доступом (ACL) с традиционной безопасностью POSIX или UNIX.

Если ваша система сообщает свой стиль безопасности как UNIX или POSIX без включения сокращенного списка ACL, эта проблема не повлияет на вас.

Для систем, использующих списки управления доступом, кэш Azure HPC должен отслеживаниь дополнительных значений, относящихся к пользователю, для управления доступом к файлам. Для этого необходимо включить кэш доступа. У пользователя нет элемента управления для включения кэша доступа, но вы можете открыть запрос в службу поддержки, чтобы запросить включение для затронутых целевых объектов хранилища в системе кэша.

## <a name="next-steps"></a>Дальнейшие действия

Если у вас возникла проблема, которая не была устранена в этой статье, отправьте запрос в [службу поддержки](hpc-cache-support-ticket.md) , чтобы получить помощь экспертов.
