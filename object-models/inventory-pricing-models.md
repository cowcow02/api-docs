# Inventory & Pricing Models

* [inventory](#inventory)
* [pricing](#pricing)
* [pricing-person](#pricing-person)
* [pricing-group](#pricing-group)

### inventory

Represents the inventory specification.

KEY | TYPE | NULL/EMPTY | DESCRIPTION
--- | --- | --- | ---
startDateTime | string | no | Start local date time for the inventory. Format: TODO
endDateTime | string | no | End local date time for the inventory. Format: TODO
availability | enum | no | Specifies the type of availability for the inventory. `enum: LIMITED, UNLIMITED, CLOSED`. Ref: [inventory.availability](#inventory--availability)
remaining | int | no | The total number of seats left which can be booked for this inventory.
pricing | [pricing](#pricing) | no | The pricing specifiction for the inventory.

##### <a name="inventory--availability"></a>`inventory.availability`

* `LIMITED`: Specifies that there is limited availability for this inventory and the remaining seats can be fetched from [inventory.remaining](#inventory)
* `UNLIMITED`: Specifies that there is unlimited availability for this inventory and can be booked any number of times.
* `CLOSED`: Specified that the inventory is closed.

### pricing

KEY | TYPE | NULL/EMPTY | DESCRIPTION
--- | --- | --- | ---
persons | array[[pricing-person](#pricing-person)] | yes | Specifies the specification for per person pricing. This is non-empty only if `priceType` for the variant of the inventory is `PER_PERSON`. Ref: [product-variant.priceType](product-models.md#product-variant)
groups | array[[pricing-group](#pricing-group)] | no | Specifies the specification for per person pricing. This is non-empty only if `priceType` for the variant of the inventory is `PER_GROUP`. Ref: [product-variant.priceType](product-models.md#product-variant)

### pricing-person

Specifies the specification for per person pricing. This is only applicable for `PER_PERSON` price types.

KEY | TYPE | NULL/EMPTY | DESCRIPTION
--- | --- | --- | ---
type | string | no | The person type. Eg: `ADULT`, `CHILD` etc.
name | string | no | The displayable name for the person. Ex: `Adult`, `Child`, `Senior`
ageFrom | int | yes | The minimum age for the type. If this not specified then there is no minimum age.
ageTo | int | no | The maximum age for the type. If this is not specified then there is no macimum age.
price | float | no | The price payable for this type.

TODO: Need to tell the type -> id, name change. Need to make this change in booking too. Report to durga.

### pricing-group

Specifies the specification for per group pricing. This is only applicable for `PER_GROUP` price types.

KEY | TYPE | NULL/EMPTY | DESCRIPTION
--- | --- | --- | ---
size | int | no | The upper bound of the size of this group. The lower bound is the next lowest size overall from this size plus 1.
price | float | no | The price payable for this type.

##### Eg:

`size=4, price=100; size=7, price=120; size=10, prize=150`

Signifies (For currency $):

`1-4 persons cost $100; 5-7 persons cost $120; 8-10 persons cost $150`