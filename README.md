# Autonomous Subscription Service

*Address:* [HREB3DFY7DCOEYTV2UGYVWLKOZZHDNHG](https://testnetexplorer.obyte.org/#HREB3DFY7DCOEYTV2UGYVWLKOZZHDNHG)
*Definer unit (testnet):* [tz7wCxXg3Jw5kleFeBhrsFgQXuWjEttRFDcmi+6hgog=](https://testnetexplorer.obyte.org/#tz7wCxXg3Jw5kleFeBhrsFgQXuWjEttRFDcmi+6hgog=)

This autonomous agent allows service providers to set up recurring subscription contracts for their customers
and charge an agreed amount periodically. The customers must consent to the contract conditions before the
service provider can start collecting money. The customers can fund their account with any amount of money which
they can withdraw at any time. Customers and service providers are also allowed to cancel the recurring contract
at any time. Service providers should listen to events of this AA corresponding their customers to get notified
if a cancellation happens.

Typical usage:

 * contract setup: service provider sends contract details
 * contract authorization: customer authorizes the contract set up by the service provider
 * customer account funding: the customer funds their account by sending bytes to AA
 * subscription fee collection: the service provider collects the recurring charges periodically
 * contract termination: the service provider or the customer cancels the contract
 * funds withdrawal: the customer withdraws the funds if they aren't planning to use the provided service anymore

## Set up a recurring contract

Triggered by the service provider to set up the recurring contract.

*Parameters*

 * `payer` - the customer's Obyte address
 * `period` - the length of the recurring period in seconds, charges can be collected once in a period
 * `amount` - the amount in bytes to be charged periodically

*Response*

 * `reference` - the recurring contract reference to be used in subsequent requests to collect subscription fees. The service provider is supposed to save this reference.

## Authorize contract

Triggered by customer to accept the recurring contract conditions. This step is to ensure the customer agrees to the same terms the service provider set up. If the customer sends more than the minimally required amount of money (typical fee + amount for the first recurring charge), then that will be added to their account balance.

The parameters required by this branch correspond to the ones the service provider set up. The service provider could make it easier this step for the customer by sending an Obyte link with pre-filled data elements and a payment addressed to this AA.

*Parameters*

 * `payee` - the Obyte address of the service provider that will collect the subscription fees
 * `period` - the length of the recurring period in seconds
 * `amount` - the amount in bytes to be charged periodically

*Response*

 * `reference` - the recurring contract reference to be used to cancel the contract. The customer is supposed to save this reference.

## Collect recurring payment

Triggered by the service provider to collect subscription fees. Since the contract details are already stored in the state variables of the AA, providing just the reference is sufficient.

*Parameters*

 * `reference` - the reference of the recurring contract to collect fees for

## Deposit money

Triggered by customer to deposit money.
 
No parameters required, the customer simply need to send some money exceeding the bounce fees.

## Withdraw money

Triggered by the customer to withdraw a specified amount of money or the whole balance to the 'trigger.address'.

*Parameters*

 * `action` - the literal `withdraw`
 * `amount` - optional, if provided it's the amount in bytes to be withdrawn, otherwise the full balance is withdrawn.

## Cancel a contract

Triggered by the customer or the service provider to cancel the contract.

*Parameters*

 * `action` - the literal `cancel`
 * `reference` - the reference of the recurring contract to be cancelled

*Response*

 * `reference
 * `status` - the new cancelled status

