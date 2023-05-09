    BSIP: <BSIP number>
    Title: Recurring Withdrawals Defined in Monthly Intervals
    Authors: Taconator
    Status: Draft
    Type: Protocol
    Created: 2018-08-15
    Discussion: https://github.com/bitshares/bitshares-core/issues/1208

# Abstract
Recurring withdrawals can currently be defined in BitShares core with respect to any periodic interval greater than the minimum block interval (currently 3 seconds). However those intervals are not intuitive for many use cases such as intervals defined in terms of Gregorian calendar months. This BitShares Improvement Proposal describes possible defintions for the periodic intervals and possible implementations such that they are compatible with the existing recurring withdrawal capability. The adoption of of this proposal will likely increase the goods and services that are accessible through BitShares by making it easier for merchants to accept payment on an intuitive montly basis.

# Motivation
Recurring withdrawals can currently be defined with respect to any periodic interval greater than the minimum block interval (currently 3 seconds). However those intervals are not intuitive for many use cases such as a desire for a monthly interval. An approximation to a monthly interval might be 30 days; with that approximation and with a starting date-time of January 1 (in the Gregorian Calendar), the next period will begin on January 31, and the subsequent period will begin on March 1 or March 2 depending on whether it is a leap year.

One comment suggested that [many use cases will need the ability to define intervals in terms of common secular measurements](https://github.com/bitshares/bitshares-ui/issues/30#issuecomment-375994404) such as "monthly" or "weekly". When intervals are defined in terms of Gregorian calendar months, the previous example would be as follows: with a starting date-time of January 1, the next period will begin on February 1, and the subsequent period will begin on March 1.

# Rationale
## Elements of the Existing Recurring Definition

The existing recurring withdrawal periods are defined by three elements:

- a [starting date-time in UTC](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L53),
- a [periodic interval defined in terms of seconds](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L58), and
- an [expiration date-time in UTC](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L60).

The periodic interval is defined in terms of seconds rather than secular periods such as calendar months. The combination of these elements imply a specific quantity of periods during which withdrawals are authorized. Note that it is possible for the last period to be a fraction of a periodic interval due to the open-ended definition of the expiration date-time.

## Definition A: Elements of the Additional Recurring Definition

The proposed recurring withdrawal periods are defined by three elements:

- a starting date-time in UTC,
- a periodic interval defined in terms of calendar units, and
- an expiration date-time in UTC.

The difference in definition is how the periodic interval is defined in terms of calendar units such as Gregorian calendar months. If the calendar unit is a Gregorian month, the starting day of the month will be the day of the month in UTC **derived from the starting-date-time**.

### Caution: Start of Recurring Month

The specified starting date-time will serve two purposes: (1) it will define the start of the recurring authorization, and (2) will implicitly define the starting day of the month **and** the starting time in UTC within that day.

If the starting day of the month is derived to be on either Day 29, 30, or 31 of a month, caution must be taken because not all months in a Gregorian calendar contain these days. One simple manner of addressing this is to prohibit the selection of these days as the start of a recurring month. Another manner is to interpret such a day as beginning on the day requested **if** that Gregorian month contains that day, and otherwise to specify the prior day in the month that is present during that calendar month (for example, a start day of 31 will become 30 when the month is April).

### Caution: Possibility of Fractional Last Period

As in the existing definition, it is possible for the last period to be a fraction of a periodic interval due to the open-ended definition of the expiration date-time.

## Definition B: Elements of the Additional Recurring Definition

The proposed recurring withdrawal periods are defined by five elements:

- a starting date-time in UTC,
- a starting day of month defined as a number between 1-28,
- a starting hour of the day in UTC defined by a number between 0 and 23 (inclusive),
- a periodic interval defined in terms of calendar units, and
- an expiration date-time in UTC.

This definition is more explicit than Definition A which might assist both users, developers, and the code. The starting day of the month and hour of the day are **explicitly defined** and need not be derived by users by performing any time calculations.

### Caution: Possibility of Fractional First Period

There are two possible interpretations for how to handle the first recurring period especially if the starting date-time in UTC does not coincide with the starting day of month or starting hour of the day in UTC. Consider the example:

- the starting date-time in UTC is January 1, 2018 14:00 UTC
- the starting day is 5 and the starting hour is 20

It is clear that future recurring periods will begin on Janury 5 20:00 UTC, February 5 20:00 UTC, March 5 20:00 UTC, etc. However an open question is how to handle the time between January 1, 2018 14:00 UTC and January 5 20:00 UTC. This could be interpreted to be a valid withdrawal period that is shorter than usual, or it could be interpreted to be an inactive period prior to the start of the "real" first period. The former approach is likely the easier to implement and it is up to the users who define the recurring withdrawal to align the definitions appropriately.

### Caution: Possibility of Fractional Last Period

As in the existing definition, it is possible for the last period to be a fraction of a periodic interval due to the open-ended definition of the expiration date-time.

## Time Zone

Starting and expiration times are currently specified in Universal Coordinated Time (UTC) zone to simplify time calculations and to avoid issues with newly defined time zones that might require hardforks in the blockchain logic. This constraint still provides a user with flexibility to define a range of times in UTC. Although this might vary relative to a local time zone over the course of a year, it still enable the creator of a recurring withdrawal definition to approximate a time in their local time zone.

# Specifications

## Specification of Intervals

The existing definitions of recurring withdrawal interval are defined in three locations:

- the [withdrawal_period_sec field of the withdraw_permission_create_operation](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/protocol/withdraw_permission.hpp#L61);
- the [withdrawal_period_sec field of the withdraw_permission_update_operation](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/protocol/withdraw_permission.hpp#L96); and
- the [withdrawal_period_sec field of the withdraw_permission_object](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L53)

These existing intervals of time are defined in seconds.

## Specification of Period Start Date-Times

The definitions of recurring withdrawal start date-times are defined in three locations:

- the [period_start_time field field of the withdraw_permission_create_operation](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/protocol/withdraw_permission.hpp#L65);
- the [period_start_time field field of the withdraw_permission_update_operation](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/protocol/withdraw_permission.hpp#L98); and
- the [period_start_time field of the withdraw_permission_object](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L58)

These existing start date-times are defined as seconds elapsed since the Unix epoch of January 1, 1970.

## Approach A: Enhance Existing Objects and Operations

One implementation approach is to enhance the existing objects and operations that are related to recurring withdrawals. The addition of the optional definition for recurring interval will involve adding conditional logic to determine which of the definitions is applicable and then implementations for the recurring monthly definition when applicable.

Substantive updates will be required to:

- [withdraw_permission_object::available_this_period()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/include/graphene/chain/withdraw_permission_object.hpp#L71)
- [operation_test2::current_period()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/tests/tests/operation_tests2.cpp#L86)
- [withdraw_permission_claim_evaluator::do_apply](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/withdraw_permission_evaluator.cpp#L104)

Moderate updates will be required to:

- [withdraw_permission_update_evaluator::do_evaluate](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/withdraw_permission_evaluator.cpp#L124)

Trivial updates will be required to:

- [withdraw_permission_update_evaluator::do_apply()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/withdraw_permission_evaluator.cpp#L139)
- [database::update_withdraw_permissions()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/db_update.cpp#L516)

One [comment noted that the existing withdrawal operations lack an "extensions" field](https://github.com/bitshares/bitshares-core/issues/1208#issuecomment-410515131) which means that the operations cannot be extended easily.  The logic in certain non-operation objects, such as withdraw_permission_object and withdraw_permission_claim_evalautor, could be enhanced to handle both the existing definition and monthly definitions, the complexity added might be confusing or error-prone both in the Core **and in client software**. Therefore a possible alternative is to make use of new operations and new internal objects.


## Approach B: Use New Objects and Operations

A new internal object (possibly called withdraw_secular_permission_object) and complementary operations could adopt most of the existing implementations and make changes that are suggested by Approach A while including the addition of an "extensions" field to future-proof this definition. Future-proofing is likely to be worthwhile as the demand for other secular intervals such as weekly or yearly might also develop.

A new index will be required to track the new object. The new index:

- will need to be defined in the new class defined in withdraw_secular_permission_object.hpp
- will need to be added to [database::initialize_indexes add_index< primary_index<withdraw_permission_index > >()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/db_init.cpp#L200)
- will need to be referenced in [database_api_impl::get_full_accounts](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/app/database_api.cpp#L722-L727)
- will need to be referenced from [database_api_impl::get_withdraw_permissions_by_giver](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/app/database_api.cpp#L2116)
- will need to be referenced from [database_api_impl::get_withdraw_permissions_by_recipient](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/app/database_api.cpp#L2137)
- will need to be referenced from [database::update_withdraw_permissions()](https://github.com/bitshares/bitshares-core/blob/58969c2a0307e32bbdee731d1f3f9a193b0d1b3f/libraries/chain/db_update.cpp#L516)


# Discussion

A design question (in contrast to the existing definition) is whether an exact quantity of periods should be explicitly defined which will result in an implied expiration date. This is related to _"Specification of Intervals"_ and _"Specification of Period Start Date-Times"_.

An implementation question is whether this secular period definitions should enhance the existing withdrawal objects and operations, or should define new objects and operations.  See _"Approach A"_ and _"Approach B"_.


# Summary for Stakeholders

The ability to define recurring withdrawals in commonly recognized intervals, such as "monthly", will likely increase the goods and services that are accessible through BitShares by making it easier for merchants to accept payment on an intuitive montly basis. The adoption of such a feature will likely also increase the usage of various BitAssets including bitCNY and bitUSD.

# Copyright

This document is placed in the public domain.

# See Also
[UI/UX for Recurring Withdrawals](https://github.com/bitshares/bitshares-ui/issues/30#issuecomment-375994404)

[General Idea: New Functionality to Realize Recurring/Scheduled Withdrawals on BitShares](https://github.com/bitshares/bitshares-core/issues/540)
