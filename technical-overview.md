# Technical Overview: Enhanced Hotel Data Pipeline

## Current Architecture

The enhanced data pipeline infrastructure is implemented across three repositories:

### 1. Data Storage (terraform-automation)
- **Enhanced BigQuery Tables**: 4 new tables for daily-level data
  - `availability_details_enhanced_staging` - Daily availability with board basis
  - `check_price_enhanced_staging` - Daily pricing with party composition
  - `availability_summary_enhanced` - Aggregated availability metrics
  - `pricing_summary_enhanced` - Aggregated pricing metrics
- **Schema Features**: Partitioned by departure date, clustered for query performance

### 2. Data Collection (search-jobs)  
- **Enhanced Pipeline Jobs**: Batch processing with configurable parameters
- **Rate Limiting**: Built-in throttling to prevent API overload
- **Change Detection**: Identifies when hotel data has changed to minimize unnecessary API calls
- **Configuration**: Supports multiple party compositions, durations, and board basis types

### 3. Data Processing (analytics-etl-transformer)
- **Aggregation Logic**: Processes daily data into business intelligence summaries
- **Metrics Calculation**: Board basis analysis, party composition insights

## Implementation Status

### ✅ Completed Components
- BigQuery schemas designed and deployed
- Data pipeline logic implemented with enhanced parameter support
- Processing jobs ready to handle daily-level data
- Test scripts validate end-to-end functionality

### ❌ Missing Component: Enhanced APIs
The pipeline is designed to call existing hotel search APIs with enhanced parameters, but these APIs currently have limitations:

**Current API Constraints:**
- `CheapestPriceRequest` has hardcoded parameters:
  ```java
  .nights(7)  // Fixed duration
  .rooms(Collections.singletonList(new Room(2)))  // Fixed party size
  .departureDateMin/Max(broad ranges)  // Not specific dates
  ```
- Responses provide monthly aggregation, not daily granularity
- No board basis information in current responses

## Required API Enhancements

### Request Model Changes
APIs need to accept configurable parameters:
- Specific departure dates (not just broad ranges)
- Multiple duration options (2, 3, 5, 7 nights)
- Different party compositions (adults/children combinations)
- Board basis filtering/requests

### Response Model Changes
APIs need to return:
- Daily-level granularity instead of monthly aggregation
- Board basis breakdowns per date/duration/party combination
- All requested parameter combinations, not just cheapest/summary

### Implementation Notes
- Current pipeline configuration already supports these parameters
- Pipeline can make multiple API calls with different parameter combinations
- Rate limiting and batching are already implemented
- Change detection minimizes API call frequency

## Deployment Considerations

### API Service Repositories
The actual API implementations are in separate repositories (not search-jobs):
- `offer-search-service` - Contains hotel search API endpoints
- `search-service` - Contains additional search functionality
- Need to coordinate changes across multiple services

### Testing Strategy
- Enhanced APIs can be implemented incrementally
- Start with daily granularity, add other parameters progressively
- Pipeline configuration already supports feature flags for gradual rollout

### Performance Impact
- Enhanced APIs will generate more granular data
- Pipeline includes throttling and batching to manage load
- BigQuery tables are optimized for increased data volume

## Integration Points

The enhanced pipeline will connect to upgraded APIs via:
1. Enhanced `CheapestPriceRequest` calls with configurable parameters
2. Processing of enhanced response data with daily granularity
3. Storage of detailed data in enhanced BigQuery schemas
4. Aggregation into business intelligence summaries

## Monitoring and Observability

Pipeline includes:
- Data quality validation at each processing stage
- Metrics on API call frequency and success rates
- Alert conditions for data freshness and completeness
- Business intelligence validation queries