---
title: Compra de reservas de Azure con pagos por adelantado o mensuales
description: Aprenda a comprar reservas de Azure con pagos por adelantado o mensuales.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: banders
ms.openlocfilehash: 77d663fa01e24acf63acd68d0b8d7cf4cc741055
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587097"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Compra de reservas con pagos mensuales

Hasta ahora, las reservas de Azure requerían el pago por adelantado. Ahora puede pagar las reservas mensualmente. A diferencia de la compra por adelantado (importe total), la opción de pago mensual divide el costo de la reserva en partes iguales cada mes del período. El costo total de las reservas por adelantado y mensuales es el mismo y no se pagan cargos adicionales por elegir el pago mensual.

El importe del pago mensual puede variar en función del tipo de cambio de su divisa en el mes en curso.

Los pagos mensuales están disponibles para:

- Máquinas virtuales
- Azure Storage
- Instancias de SQL Database
- SQL Data Warehouse
- Cosmos DB
- Impuesto sobre el timbre de App Service
- Disco administrado
- Explorador de datos de Azure
- Azure Database for MariaDB, MySQL y PostgreSQL
- Azure VMware Solution by CloudSimple


Realice reservas en [Azure Portal](https://portal.azure.com/?Microsoft_Azure_Reservations_EnableMultiCart=true&amp;paymentPlan=true#blade/Microsoft_Azure_Reservations/CreateBlade).

![Ejemplo que muestra una reserva](./media/monthly-payments-reservations/purchase-reservation.png)

Al realizar una reserva puede ver el calendario de pagos. Haga clic en **Ver el calendario de pagos completo**.

![Ejemplo que muestra el calendario de pagos de las reservas](./media/monthly-payments-reservations/prepurchase-schedule.png)

Para ver el calendario de pagos después de la reserva, selecciónela, y haga clic en el **id. de pedido de reserva** y en la pestaña **Pagos**.

## <a name="view-payments-made"></a>Visualización de los pagos realizados

Puede ver los pagos realizados mediante las API, los datos de uso y el análisis de costos. En el caso de las reservas de pago mensual, el valor de la frecuencia aparece como **periódico** en los datos de uso y en Reservation Charges API. En el caso de las reservas pagadas por adelantado, el valor se muestra como **OneTime**.

El análisis de costos muestra las compras mensuales en la vista predeterminada. Aplique el filtro **compra** en **Charge type** (Tipo de cargo) y **periódico** en **Frecuencia** para ver todas las compras. Para ver solo las reservas, aplique un filtro de **Reserva**.

![Ejemplo que muestra los costos de las reservas en el análisis de costos](./media/monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Cambio al pago mensual en la renovación

Al renovar una reserva, puede cambiar la frecuencia de facturación a mensualmente.

## <a name="exchange-and-refunds"></a>Cambios y reembolsos

Al igual que otras reservas, con la facturación mensual se pueden reembolsar o cambiar. Actualmente puede enviar una solicitud de soporte técnico para iniciar un cambio o reembolso de una reserva adquirida con facturación mensual.

Al cambiar una reserva pagada mensualmente, el costo total de la nueva compra debe ser mayor que los pagos restantes que se cancelan de la reserva devuelta. No hay ningún otro límite ni tarifa por los cambios. Puede cambiar una reserva pagada por adelantado para comprar una nueva que se facture mensualmente. Sin embargo, el valor de la duración de la nueva reserva debe ser mayor que el prorrateado de la reserva que se va a devolver.

Si cancela una reserva de pago mensual, es posible que Microsoft aplique una cuota de cancelación del 12 % a los pagos confirmados cancelados. Sin embargo, Microsoft no cobra actualmente la penalización. Los pagos confirmados cancelados se acumulan en el límite de reembolso de 50 000 USD. Si se cobra una penalización por cancelación, el límite de reembolso no resulta afectado.

Para más información sobre los cambios y los reembolsos, consulte [Autoservicio de cambios y reembolsos de reservas de Azure](exchange-and-refund-azure-reservations.md).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las reservas, consulte [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
- Para información sobre las tareas que debe realizar antes de adquirir una reserva, consulte [Determinación del tamaño adecuado de una máquina virtual antes de su adquisición](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).
