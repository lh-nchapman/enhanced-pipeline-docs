# Business Overview: Enhanced Hotel Data Collection

## Current Situation

Right now, we can only see hotel data in monthly summaries. For example, we might know the average price for hotels in "March 2025" but we can't see differences between March 1st vs March 15th vs March 30th.

## What We Want to Change

### From Monthly to Daily Data
- **Instead of**: "Hotel ABC averages £500 per person in March"
- **We want**: "Hotel ABC is £400 on March 1st, £600 on March 15th, £450 on March 30th"

### From One Package Type to Multiple Options
- **Instead of**: Only 7-night packages for 2 adults
- **We want**: 2,3,5,7 night options for both couples (2 adults) and families (2 adults + 1 child)

### From Basic Pricing to Meal Plan Details
- **Instead of**: One price per hotel
- **We want**: Separate pricing for Room Only, Bed & Breakfast, Half Board, All Inclusive

## Business Benefits

### Better Pricing Decisions
- See which specific dates are more expensive
- Identify patterns like "weekends cost 20% more"
- Price our packages more competitively on specific dates

### Target Different Customers
- Understand how families book differently than couples
- Create targeted offers for different party types
- Optimize marketing for different customer segments

### Maximize Meal Plan Revenue
- See which board basis options sell best on which dates
- Price meal plans competitively
- Identify upselling opportunities

## What's Ready vs What's Needed

### ✅ What's Built and Ready
- Data processing pipeline that can handle daily-level data
- Database tables designed for enhanced data storage
- Analysis tools ready to work with detailed data

### ❌ What's Missing (The Blocker)
The hotel search systems currently only provide monthly summary data. To get the detailed daily data we want, the search APIs need to be enhanced to:

1. Accept requests for specific departure dates
2. Handle different duration options (2,3,5,7 nights)
3. Support different party compositions (couples vs families)
4. Return board basis breakdowns in responses

## Timeline Impact

**Best Case**: If API enhancements start immediately, we could have enhanced data collection in 8-12 weeks
**Realistic**: Need to coordinate with API development teams and plan the work, likely 3-6 months

## Questions for Engineering Teams

1. Where are the hotel search API implementations located?
2. Is enhanced API development already planned or in progress?
3. What's the estimated effort to add daily granularity and flexible parameters?
4. Can this be implemented incrementally (start with daily dates, add other features later)?

## Success Criteria

We'll know it's working when we can:
- See different hotel prices for consecutive departure dates
- Compare 2-night vs 7-night pricing for the same hotel and date
- View separate pricing for couples vs families
- Access board basis options (RO, BB, HB, AI) for each hotel/date combination