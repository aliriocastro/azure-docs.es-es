---
title: 'Serie DC: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie DC.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: fd8f01f0fc7180d271404ffee4496ff9cbac7222
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2020
ms.locfileid: "78205932"
---
# <a name="preview-dcv2-series"></a>Vista previa: Serie DCv2


La serie DCv2 puede ayudar a proteger la confidencialidad y la integridad de los datos y del código mientras se están procesando en la nube pública. Estas máquinas están respaldadas por la última generación del procesador Intel XEON E-2288G con la tecnología SGX. Con la tecnología Intel Turbo Boost, estas máquinas pueden llegar hasta 5,0 GHz. Las instancias de la serie DCv2 permiten a los clientes compilar aplicaciones seguras basadas en enclave para proteger su código y sus datos mientras se usan.

Entre los casos de uso de ejemplo se incluyen el uso compartido de datos confidenciales entre varias partes, la detección de fraudes, el blanqueo de dinero, la cadena de bloques, el análisis de uso confidencial, el análisis de la inteligencia y el aprendizaje automático confidencial.

Premium Storage: Admitido*

Almacenamiento en caché de Premium Storage: Admitido*

Migración en vivo: No compatible

Actualizaciones con conservación de memoria: No compatible

*Excepto para Standard_DC8_v2



| Size             | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC / ancho de banda de red esperado (MBps) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2                                            |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                            | 3200/48                                   | 2                                            |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                            | 6400/96                                   | 2                                            |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128 (172)                                                         | 12800/192                                 | 2                                            |

- Las máquinas virtuales de la serie DCv2 son [VM de 2.ª generación](./linux/generation-2.md#creating-a-generation-2-vm) y solo admiten imágenes de `Gen2`.
- Actualmente solo disponible en Sur de Reino Unido.
- Generación anterior de máquinas virtuales de proceso confidencial: [Serie DC](sizes-previous-gen.md)
- Creación de máquinas virtuales DCv2 con Azure Portal [Creación de una máquina virtual: Azure Portal](./linux/quick-create-portal.md)



## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
