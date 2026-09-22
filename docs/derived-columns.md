# ETL Derived Columns

This document describes the calculated columns used during the SSIS ETL process for loading the sales fact table.

## GrossSales

Gross value of a sales line before discount.

```text
Quantity * UnitPrice

SSIS expression:

(DT_NUMERIC,18,4)((DT_NUMERIC,18,4)Quantity * (DT_NUMERIC,18,4)UnitPrice)

EffectiveDiscountAmount

Actual monetary value of the applied discount.

GrossSales - TotalPrice

SSIS expression:

(DT_NUMERIC,18,4)(((DT_NUMERIC,18,4)Quantity * (DT_NUMERIC,18,4)UnitPrice) - (DT_NUMERIC,18,4)TotalPrice)
EffectiveDiscountPct

Percentage of the gross value that was discounted.

EffectiveDiscountAmount / GrossSales

SSIS expression:

(DT_NUMERIC,18,6)(((DT_NUMERIC,18,6)((DT_NUMERIC,18,4)Quantity * (DT_NUMERIC,18,4)UnitPrice) - (DT_NUMERIC,18,6)TotalPrice) / (DT_NUMERIC,18,6)((DT_NUMERIC,18,4)Quantity * (DT_NUMERIC,18,4)UnitPrice))
UnitNetPrice

Net price paid per individual unit after discount.

TotalPrice / Quantity

SSIS expression:

(DT_NUMERIC,18,4)(Quantity > 0 ? (DT_NUMERIC,18,4)TotalPrice / (DT_NUMERIC,18,4)Quantity : 0)
HasDiscount

Boolean indicator showing whether a discount was applied.

GrossSales > TotalPrice

SSIS expression:

(DT_BOOL)((DT_NUMERIC,18,4)Quantity * (DT_NUMERIC,18,4)UnitPrice > (DT_NUMERIC,18,4)TotalPrice)