# Enhanced Hotel Data Pipeline Project

⚠️ **TEMPORARY REPOSITORY** - This is a temporary documentation repository for sharing project details. Please save any needed documents locally and this repository will be deleted after review period.

## Overview

This repository contains documentation for enhancing an **existing hotel data pipeline** at Love Holidays. We currently collect hotel availability and pricing data at monthly levels and want to enhance this to daily granularity.

**Our Challenge**: Scope whether this enhancement is technically feasible and estimate implementation effort.

## Documents

1. **[Project Overview](project-overview.md)** - What we are trying to achieve
2. **[Technical Overview](technical-overview.md)** - For engineers working on implementation  
3. **[Business Overview](business-overview.md)** - For non-technical stakeholders
4. **[Engineering Requirements](engineering-requirements.md)** - Specific changes needed from engineering teams

## Current Status

**Existing Pipeline**: Successfully collects monthly hotel data into:
- `lh-data-pipeline-prod.availability_stats.availability_details`
- `lh-data-pipeline-prod.availability_stats.check_price`

**Enhancement Approach**: Piggyback off the same processes that power the existing tables to create enhanced versions with daily granularity:
- `availability_details_enhanced_staging`
- `check_price_enhanced_staging` 
- `availability_summary_enhanced`
- `pricing_summary_enhanced`

**Key Question**: Can the underlying hotel search APIs be enhanced to provide the detailed data we need?

## What We're Trying to Achieve

**Current Pipeline Collects**:
- Monthly aggregated hotel data
- 7-night packages only  
- 2-adult bookings only
- Basic pricing without meal plan details

**Enhanced Pipeline Would Collect**:
- Daily departure date data
- Multiple durations (2,3,5,7 nights)
- Different party types (couples vs families)
- Board basis breakdowns (Room Only, Bed & Breakfast, etc.)

**The Technical Challenge**: We want to leverage the existing processes that already power our monthly data collection, but enhance the underlying APIs to provide more detailed data that we can process into daily granularity tables.

## Example: Daily vs Monthly Data

**Current Monthly Data**:
```
Hotel ABC - March 2025: £400 cheapest price, 85% availability
```

**Proposed Daily Data**:
```
Hotel ABC - March 15, 2025:
- 2 adults, 7 nights: £450/person (Room Only), £520/person (Half Board)
- 2 adults + 1 child, 7 nights: £380/person (Room Only), £445/person (Half Board)  
- 2 adults, 3 nights: £485/person (Room Only), £550/person (Half Board)
```