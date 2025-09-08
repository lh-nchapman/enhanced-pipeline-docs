# Engineering Requirements: Enhanced Hotel Search APIs

## Problem Statement

The enhanced data pipeline is ready but cannot function because current hotel search APIs provide only monthly aggregated data. We need APIs that can provide daily-level data with flexible parameters.

## Required Changes

### 1. Locate API Service Repositories

**Current Issue**: The `search-jobs` repository contains API clients, not implementations.

**Action Needed**: Identify repositories containing:
- Hotel search API endpoints (`/api/v1/cheapest`, `/api/v2/availability`, etc.)
- Likely repositories: `offer-search-service`, `search-service`

### 2. Request Model Enhancements

**Current Limitation**: APIs have hardcoded parameters
```java
// Current hardcoded values in CheapestPriceRequest
.nights(7)                    // Fixed 7-night duration
.rooms(Collections.singletonList(new Room(2)))  // Always 2 adults, 0 children
.departureDateMin(LocalDate.now().toString())   // Broad date ranges
```

**Required Changes**:
- Remove hardcoded values, accept parameters from request
- Support multiple duration options per request (2, 3, 5, 7 nights)
- Support different party compositions (2+0, 2+1 with child age 8)
- Accept specific departure date requests, not just broad ranges

### 3. Response Model Enhancements

**Current Limitation**: APIs return monthly cheapest price only
```java
// Current response granularity
AvailabilityDetailedResponse {
    LocalDate departureMonth;  // Monthly level only
    Float availability;        // Basic percentage
}
```

**Required Changes**:
- Return daily-level data instead of monthly cheapest price only
- Include board basis information in responses (RO, BB, HB, AI)
- Provide data for all requested parameter combinations
- Return structured data that supports party composition and duration variants

### 4. Specific API Endpoint Changes

#### `/api/v1/cheapest`
- **Current**: Returns single cheapest price with hardcoded parameters
- **Needed**: Return prices for requested date/duration/party combinations
- **Response**: Include board basis options, not just cheapest overall price

#### `/api/v2/availability-details`  
- **Current**: Returns monthly availability percentage
- **Needed**: Return daily availability with board basis breakdown
- **Response**: Boolean availability per board basis type per specific date

#### `/api/v2/price-details`
- **Current**: Returns monthly price ranges
- **Needed**: Return daily pricing with party composition and board basis variants
- **Response**: Structured pricing data per date/duration/party/board basis combination

### 5. Implementation Approach

#### Phase 1: API Parameter Flexibility
1. Remove hardcoded values from request processing
2. Accept configurable duration, party composition, date parameters
3. Maintain backward compatibility with current clients

#### Phase 2: Response Granularity
1. Modify backend processing to calculate daily-level data
2. Update response models to include board basis information
3. Return data for all requested parameter combinations

#### Phase 3: Integration
1. Update pipeline configuration to use enhanced APIs
2. Validate enhanced data collection and processing
3. Monitor performance impact and optimize as needed

### 6. Technical Specifications

#### Request Parameters to Support
```yaml
# Example enhanced request
party-configurations:
  - adults: 2, children: 0
  - adults: 2, children: 1, child-ages: [8]
duration-nights: [2, 3, 5, 7]
departure-dates: ["2025-03-15", "2025-03-16", "2025-03-17"]
board-basis-types: ["RO", "BB", "HB", "AI"]
```

#### Response Data to Include
```java
// Example enhanced response
EnhancedPriceResult {
    Long masterId;
    LocalDate departureDate;    // Specific date, not month
    Integer nights;             // Duration
    Integer adults;             // Party composition
    Integer children;
    String boardBasis;          // RO, BB, HB, AI
    Double pricePerPerson;
    Boolean available;
}
```

### 7. Timeline and Resource Estimate

**Development Effort**: 6-12 weeks
- API modifications: 3-4 weeks
- Backend data processing updates: 2-3 weeks  
- Testing and integration: 2-3 weeks
- Performance optimization: 1-2 weeks

**Required Resources**:
- Backend API developers (2-3 engineers)
- Database/data processing specialists (1-2 engineers)
- QA/testing resources (1-2 testers)

### 8. Success Criteria

#### Functional Requirements
- APIs accept specific departure dates instead of broad ranges
- APIs return data for multiple durations (2,3,5,7 nights) in single request
- APIs handle different party compositions (2+0, 2+1)
- APIs provide board basis breakdown in responses

#### Performance Requirements  
- Response time impact <50% increase from current baseline
- Support for batch requests with multiple parameter combinations
- Rate limiting appropriate for increased API call complexity

#### Integration Requirements
- Enhanced pipeline successfully connects to new API endpoints
- Daily-level data flows through to BigQuery tables
- Business intelligence queries return expected granular results

### 9. Risk Mitigation

**Backward Compatibility**: Implement as new API versions (/api/v3/) or optional parameters
**Performance Impact**: Implement caching and optimize database queries for daily-level data
**Rollback Strategy**: Maintain existing APIs during transition period
**Testing Strategy**: Comprehensive integration testing with enhanced pipeline before production deployment