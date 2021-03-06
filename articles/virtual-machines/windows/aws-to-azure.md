---
title: Перемещение экземпляра EC2 Windows AWS в Azure
description: Перемещение экземпляра виртуальной машины Windows EC2 из Amazon Web Services (AWS) на виртуальную машину Azure.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 59d1bf08c0680d222710b55c6d6bdb4d5745da56
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2020
ms.locfileid: "82084521"
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-an-azure-virtual-machine"></a>Перемещение виртуальной машины Windows из Amazon Web Services (AWS) на виртуальную машину Azure

При оценке виртуальных машин Azure для размещения рабочих нагрузок можно экспортировать существующий экземпляр виртуальной машины Windows EC2 из Amazon Web Services (AWS) и передать виртуальный жесткий диск (VHD) в Azure. После передачи VHD вы можете создать на его основе виртуальную машину в Azure. 

В этой статье описывается перемещение одной виртуальной машины из AWS в Azure. Сведения о том, как переместить масштабируемые виртуальные машины из AWS в Azure, см. в статье [Перенос виртуальных машин из Amazon Web Services (AWS) в Azure с помощью Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-the-vm"></a>Подготовка виртуальной машины 
 
В Azure можно передавать как универсальные, так и специализированные виртуальные жесткие диски. Для обоих типов требуется предварительная подготовка виртуальной машины к экспорту из AWS. 

- **Универсальный виртуальный жесткий диск**. С универсального VHD с помощью Sysprep удалены все сведения вашей личной учетной записи. Если вы планируете использовать виртуальный жесткий диск в качестве образа для создания виртуальных машин, то выполните следующие действия: 
 
    * [Подготовьте виртуальную машину Windows](prepare-for-upload-vhd-image.md).  
    * Сделайте виртуальную машину универсальной с помощью Sysprep.  

 
- **Специализированный виртуальный жесткий диск**. На специализированном VHD сохраняются учетные записи пользователей, приложения и другие данные о состоянии исходной виртуальной машины. Если вы планируете использовать виртуальный жесткий диск "как есть" для создания виртуальной машины, то необходимо выполнить следующие действия:  
    * [Подготовка виртуального жесткого диска Windows к отправке в Azure](prepare-for-upload-vhd-image.md). **Не выполняйте** подготовку виртуальной машины к использованию с помощью Sysprep. 
    * Удалите все гостевые инструменты и агенты виртуализации, которые установлены на виртуальной машине (т. е. инструменты VMware). 
    * Убедитесь, что виртуальная машина настроена на получение IP-адреса и параметров DNS через DHCP. Таким образом, сервер будет получать IP-адрес в виртуальной сети при запуске.  


## <a name="export-and-download-the-vhd"></a>Экспорт и скачивание VHD-файла 

Экспортируйте экземпляр EC2 на VHD в контейнере Amazon S3. Выполните действия, описанные в документации Amazon в статье [Exporting an Instance as a VM Using VM Import/Export](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) (Экспорт экземпляра виртуальной машины с помощью службы импорта и экспорта виртуальных машин) и выполните команду [create-instance-export-task](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html), чтобы экспортировать экземпляр EC2 в VHD-файл. 

Экспортированный VHD-файл сохраняется в указанном контейнере Amazon S3. Базовый синтаксис для экспорта виртуального жесткого диска приведен ниже, просто замените текст заполнителя в \<квадратных скобках> информацией.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

По завершении экспорта VHD следуйте инструкциям в разделе о [How Do I Download an Object from an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) (Как скачать объект из контейнера S3?), чтобы скачать VHD-файл из контейнера S3. 

> [!IMPORTANT]
> В AWS взимается плата за передачу данных при скачивании VHD-файла. Дополнительные сведения см. на странице [Цены на Amazon S3](https://aws.amazon.com/s3/pricing/).


## <a name="next-steps"></a>Дальнейшие шаги

Теперь можно передать VHD в Azure и создать виртуальную машину. 

- Если вы запустили Sysprep на исходной виртуальной машине, чтобы **сделать ее универсальной** перед экспортом, см. статью о [передаче универсального виртуального жесткого диска и его использовании для создания виртуальных машин в Azure](upload-generalized-managed.md)
- Если вы не запускали Sysprep перед экспортом, VHD считается **специализированным**. См. статью [Создание виртуальной машины из специализированного диска](create-vm-specialized.md).

 
