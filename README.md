# Enhanced Hotel Data Pipeline Project

## Overview

This repository contains documentation for enhancing Love Holidays' hotel availability and pricing data collection from monthly aggregation to daily granularity.

## Documents

1. **[Project Overview](project-overview.md)** - What we are trying to achieve
2. **[Technical Overview](technical-overview.md)** - For engineers working on implementation  
3. **[Business Overview](business-overview.md)** - For non-technical stakeholders
4. **[Engineering Requirements](engineering-requirements.md)** - Specific changes needed from engineering teams

## Current Status

**What's Ready:** Data pipeline infrastructure, BigQuery schemas, and processing logic
**What's Needed:** Enhanced APIs to provide daily-level data with board basis and party composition support

## Quick Summary

We want to move from:
- **Current**: Monthly aggregated hotel data, 7-night packages, 2-adult bookings only
- **Target**: Daily departure date data, flexible durations (2,3,5,7 nights), couples vs families, board basis breakdowns

The pipeline is ready, but the underlying hotel search APIs need enhancement to provide this detailed data.