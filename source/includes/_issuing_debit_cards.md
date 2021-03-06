# Issuing Debit Cards
> **Endpoints**

```shell
  POST /v1/customer/cards
  POST /v1/customer/cards/{{CARD_ID}}/activate
  POST /v1/customer/cards/{{CARD_ID}}/suspend
  POST /v1/customer/cards/rules
  POST /v1/customer/cards/rules/{{CARD_RULE_ID}}/disable
  GET /v1/customer/cards/{{CARD_ID}}
  GET /v1/customer/cards/{{CARD_TOKEN}}
  GET /v1/customer/cards
  GET /v1/customer/cards/{{CARD_ID}}/image
  GET /v1/customer/cards/{{CARD_ID}}/pin
  GET /v1/customer/cards/rules
  GET /v1/customer/cards/rules/{{CARD_RULE_ID}}
  PUT /v1/customer/cards/rules/{{CARD_RULE_ID}}
  PUT /v1/customer/cards/rules/{{CARD_RULE_ID}}

```

  - The **Railsbank Debit Card** solution allows our customers to issue **Mastercard Debit Cards** against Railsbank ledgers with just a few easy API calls, exercising the **Issue Cards** cabability.
  - Cards can be both virtual and physical, and - quite uniquely - a virtual card can be transformed into a physical card while retaining it's **card number (PAN)**.
  - The card design can also be customised to your taste.
  - Due to the limits of **PlayLive** and the beauty of the innovation, this cabalility is only available for customers with a **Live API Key**.
  - Customers with a **Live API Key** must have also purchased the `Railsbank-Debit-Card-1` partner product.
  - This can easily be done by contacting [support](mailto:support@railsbank.com).


## Issue a Virtual Card

> **Example Request**

  ```shell
    --request POST "https://live.railsbank.com/v1/customer/cards"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourapikey>>"
    --data
      "{
        "ledger_id": "{{LEDGER_ID}}",
        "card_programme": "{{Railsbank_Card_Programme}}"
        "card_type": "virtual",
        "card_design": "{{design_one}}",
      }"

  ```
  > **Example Response**

  ```shell
  {
    "card_id": "6630b391-c5ce-46c1-9d23-a82a9e27f82d"
  }
  ```
  >  **Example Curl Request**

  ```shell
    curl -X POST "https://beta-playlive.railsbank.com/v1/customer/cards" -H "accept: application/json" -H "Authorization: API-Key <<yourAPI-Key>>" -H "Content-Type: application/json" -d "{ \"ledger_id\": \"{{enduser_GBPledger_id}}\", \"card_type\": \"virtual\", \"card_design\": \"design one\", \"card_programme\": \"{{GBPcard_programme}}\"}"
  ```

  `POST "https://live.railsbank.com/v1/customer/cards"`

  - Once you've created your **card-holding** enduser, carefully go through our Issue a ledger tutorial - it'll only take a minute or so - to create the bank account to which your card is going to be attached.
  - When issuing the ledger, make sure to do so in the **Live** environment with your **Live API Key** and the `enduser_id` of the **card-holding** enduser you've created.
  - For customers with a **Live API Key**, the **Railsbank-Debit-Card** capability is available for testing in the **Play** environment. Please [email our Business Development Team](mailto:support@railsbank.com) if you want Live Keys.
  - The type of enduser holding the ledger must be the same that defined in the card programme. That is, if the holder of the ledger is an individual, the card programme needs to be for individuals, and if the holder is a corporate, the card programme needs to be for corporates.


| Attribute                                    | Description                   |
|:---------------------------------------------|:------------------------------|
| `ledger_id` <br> _string(UUID)_, required    | The ledger that the card will be attached to |
| `card_type` <br> _string_, required          | The type of card, in this instance: `virtual` |
| `card_design` <br> _string_, required        | The custom card design you have chosen |
| `card_programme` <br> _string_, required     | The name of the card programme you want your card to be associated with |
| `card_rules` <br> _array of UUIDs_, optional | Customizable rules associated with the card |

## Issue a Physical Card
> **Example Request**

```shell
  --request POST "https://live.railsbank.com/v1/customer/cards"
  --header "Content-Type: application/json"
  --header "Accept: application/json"
  --header "Authorization: API-Key <<yourapikey>>"
  --data
  "{
    "ledger_id": "{{enduser_GBPledger_id}}",
    "card_carrier_type": "standard",
    "card_delivery_name": "John Smith",
    "card_type": "physical",
    "card_delivery_method": "dhl",
    "card_design": "{{design_one}}",
    "card_delivery_address": {
      "address_region": "England",
      "address_iso_country": "GBR",
      "address_number": "35",
      "address_postal_code": "W1 4AQ",
      "address_refinement": "First Floor",
      "address_street": "John Street",
      "address_city": "London"
    },
    "card_programme": "{{GBPcard_programme}}"
  }"

```
  > **Example Response**

```shell
{
  "card_id": "6630b391-c5ce-46c1-9d23-a82a9e27f82d"
}
```
>  **Example Curl Request**

```shell
  curl -X POST "https://beta-playlive.railsbank.com/v1/customer/cards" -H "accept: application/json" -H "Authorization: API-Key wotzuvu60fr9y7z1o94heszi2geu8np8#kmkd4uvdmrg864ce9n4zlyygvf86opbwqddgmsdrobgqp4dlzf3thynck6qm7gso" -H "Content-Type: application/json" -d "{\"ledger_id\": \"{{enduser_GBPledger_id}}\", \"card_carrier_type\": \"standard\", \"card_delivery_name\": \"John Smith\", \"card_type\": \"physical\", \"card_delivery_method\": \"dhl\", \"card_design\": \"{{design_one}}\", \"card_delivery_address\": { \"address_region\": \"England\", \"address_iso_country\": \"GBR\", \"address_number\": \"35\", \"address_postal_code\": \"W1 4AQ\", \"address_refinement\": \"First Floor\", \"address_street\": \"John Street\", \"address_city\": \"London\" }, \"card_programme\": \"{{GBPcard_programme}}\" }"
```
`POST "https://live.railsbank.com/v1/customer/cards"`

  - Physical cards require a little more data than virtual cards.
  - Once you've created your **card-holding** enduser, carefully go through our Issue a ledger tutorial - it'll only take a minute or so - to create the bank account to which your card is going to be attached.
  - When issuing the ledger, make sure to do so in the **Live** environment with your **Live API Key** and the `enduser_id` of the **card-holding** enduser you've created.
  - For customers with a **Live API Key**, the **Railsbank-Debit-Card** capability is available for testing in the **Play** environment. Please [email our Business Development Team](mailto:support@railsbank.com) if you want Live Keys.
  - The type of enduser holding the ledger must be the same that defined in the card programme. That is, if the holder of the ledger is an individual, the card programme needs to be for individuals, and if the holder is a corporate, the card programme needs to be for corporates.

| Attribute                                                           | Description |
|:--------------------------------------------------------------------|:-------|
| `ledger_id` <br> _string(UUID)_, required                           | The ledger that the card will be attached to |
| `card_type` <br> _string_, required                                 | The type of card, in this instance: `physical` |
| `card_design` <br> _string_, required                               | The custom card design you have chosen |
| `card_programme` <br> _string_, required                            | The name of the card programme you want your card to be associated with |
| `card_rules` <br> _array of UUIDs_, optional                        | Customizable rules associated with the card |
| `qr_code_content` <br> _string_, optional                           | The string to be encoded in the auto-generated QR code on a physical card. For example, a URL, SMS message, in app action etc. |
| `name_on_card` <br> _string_, required                              | The card holder's name (to be printed onto the card) <br> _ISO basic Latin Alphabet; up to 21 characters including spaces_ |
| `card_delivery_name` <br> _string_, required                        | The name of the entity that the card will be addressed to when delivered |
| `card_delivery_method` <br> _string_, required                      | The shipping method used to deliver the card, if delivery address is outside the UK, it must be `international-mail` <br> _Allowed Values_: `dhl`, `standard-first--class`, `international-mail` |
| `card_carrier_type` <br> _string_, required                         | The carrier type to be used for the card when delivered <br> _Allowed Values: `standard`, `renewal`, `replacement` |
| `card_delivery_address` <br> _object_, required                     | The address to which the card will be delivered to |
| `card_delivery_address.address_city` <br> _string_, required        | City that enduser lives in |
| `card_delivery_address.address_iso_country`<br> _string_, required  | Country code <br>  _ISO 8601 format_ |
| `card_delivery_address.address_number` <br> _string_, required      | House or building number on street |
| `card_delivery_address.address_postal_code` <br> _string_, required | Postal or zip code |
| `card_delivery_address.address_refinement` <br> _string_, optional  | Extra detail, e.g. house name |
| `card_delivery_address.address_region` <br> _string_, required      | Region in country |
| `card_delivery_address.address_street` <br> _string_, required      | Street the enduser lives on |

## Fetch a Card

  > **Example Request**

  ```shell
    --request GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}"
  	--header "Content-Type: application/json"
  	--header "Accept: application/json"
  	--header "Authorization: API-Key <<yourliveapikey>>"
  ```
  > **Example Response**

  ```shell
  {
    "card_id": "8763b5df-a61c-4f77-9724-41ca9cde3654",
    "card_status": "card-status-created",
    "card_program": "Railsbank Card Program"
    "customer_id": "c91b339e-57d7-41ea-a805-8966ce8fe4ed",
    "partner_product": "Railsbank-Debit-Card-1",
    "ledger_id": "6630b391-c5ce-46c1-9d23-a82a9e27f82d",
    "temp_card_image_url": "237564rh43ht43"
  }
  ```
  `GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}"`

  - Fetching a card is much like 'fetching' anything else with the API.
  - In this case, all you need to know is the `card_id`.
  - The possible statuses of the card can be seen below.

### Card Statuses
| Message                           | Description                              |
|:----------------------------------|:-----------------------------------------|
| `card-status-created`             | The card has been successfully created.  |
| `card-status-suspending`          | The card is in the process of being suspended. Use the `/wait` parameter to wait until it is in a REST state (in this case, suspended). |
| `card-status-suspended`           | The card has been suspended by the enduser. You will receive a `type: card-suspended` webhook. |
| `card-status-awaiting-activation` | The card has been created but not activated. You will receive a `type: card-awaiting-activation` webhook. |
| `card-status-activating`          | The card is being activated. Use the `/wait` parameter to wait until it is in a REST state (in this case, activated). |
| `card-status-active`              | The card is active and ready to be used. You will receive a `type: card-activated` webhook. |
| `card-status-failed`              | The card has failed to create for one of the reasons below. You will receive a `type: card-failed` webhook. |
| Possible failure reasons:         |                                          |
| `card-not-active`                 | Set when card status is not equal to active. |
| `contact-support`                 | Set when we get recommendation to decline from paymentology. |
| `fx-issue`                        | Returned in one of these cases: contributed-price-not-set, contributed-price-suspended or contributed-price-expired. |
| `card-rules-breached`             | Added if any of the card rules are breached. |
| `partner-error`                   | When an error occurs on the partner side. |
| `insufficient-funds`              | When we have insufficient funds in customer card ledger. |

## Fetch a Card using its token

> **Example Request**

  ```shell
    --request GET "https://live.railsbank.com/v1/customer/cards/{{card_token}}"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```

  > **Example Response**

  ```shell
  {
      "ledger_id": "5cda9171-f2e3-4442-82f1-884c877df9e5",
      "card_token": "473894665",
      "card_id": "5cdab638-ad74-47c3-a918-15e1876dcfcf",
      "card_type": "virtual",
      "card_rules": [],
      "created_at": "2019-05-14T12:36:08.493Z",
      "partner_product": "Railsbank-Debit-Card-1",
      "card_design": "design one",
      "card_status": "card-status-awaiting-activation",
      "card_programme": "rbbe-313-gbp-person"
  }
  ```
  `GET "https://live.railsbank.com/v1/customer/cards/{{card_token}}"`

  - A few seconds after a card is created, a token is assigned to it.
  - This token can also be used to fetch the card.
  - If you are fetching the card using this endpoint after generation you will need to wait a few seconds before the token is added to the card, at which point you must fetch the card using the `card_id` in order to discover the token.
  - The card token can be used to validate physical cards. It will be nine integers on the back of the card, so when your customer receives the card, before you activate it, you can check that they are using the correct card by checking the token number.

## Fetch all of your cards
 > **Example Request**

  ```shell
    --request GET "https://live.railsbank.com/v1/customer/cards"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```
> **Example Response**

  ```shell
    {
      "card_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8",
      "card_status": "card-status-created",
      "customer_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8",
      "partner_product": "Railsbank-Debit-Card-1",
      "ledger_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8"
    },
    {
      "card_id": "8763b5df-a61c-4f77-9724-41ca9cde3654",
      "card_status": "card-status-active",
      "customer_id": "6630b391-c5ce-46c1-9d23-a82a9e27f82d",
      "partner_product": "Railsbank-Debit-Card-1",
      "ledger_id": "6630b391-c5ce-46c1-9d23-a82a9e27f82d"
    },
    {
      "card_id": "c91b339e-57d7-41ea-a805-8966ce8fe4ed",
      "card_status": "card-status-disabled",
      "customer_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8",
      "partner_product": "Railsbank-Debit-Card-1",
      "ledger_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8"
    }
  ```

  `GET "https://live.railsbank.com/v1/customer/cards"`

  - If you want to see the details of all the cards you have issued, then this powerful endpoint is for you.
  - You can filter the cards to the `holder_id`, allowing you to see the cards attached to a specific enduser, for instance.
    - You can also filter by `ledger_id`, allowing you to see the cards attached to a ledger.
    - You can specify both the `ledger_id` and the `holder_id`.

### Parameters

| Parameter                        | Description                               |
|:---------------------------------|:------------------------------------------|
| `holder_id` <br> _string_        | The UUID of the card(s) holder, an enduser or the customer. |
| `ledger_id` <br> _string_        | The UUID of the ledger to which the returned cards are attached. |
| `starting_at_date` <br> _string_ | Any cards created before this date will not be shown in the array. <br> _YYYY-MM-DD_ |
| `items_per_page` <br> _integer_  | The number of items returned in each page. A hyperlink to the next page will be found in the response headers. |
| `last_seen_id` <br> _string_     | The cards returned will have all been created after the card whose UUID is the `last_seen_id`. |
| `from_date` <br> _string_        | The date that the list of cards will start from. |
| `end_date` <br> _string_         | The date that the list of cards will end at. |


## Activate a Card
  > **Example Request**

  ```shell
    --request POST "https://live.railsbank.com/v1/customer/cards/{{card_id}}/activate"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```
  > **Example Response. Note: `status-active`.**

  ```shell
  {
      "ledger_id": "5cda9171-f2e3-4442-82f1-884c877df9e5",
      "card_token": "473894665",
      "card_id": "5cdab638-ad74-47c3-a918-15e1876dcfcf",
      "card_type": "virtual",
      "card_rules": [],
      "created_at": "2019-05-14T12:36:08.493Z",
      "partner_product": "Railsbank-Debit-Card-1",
      "card_design": "design one",
      "card_status": "card-status-active",
      "card_programme": "rbbe-313-gbp-person"
  }
  ```
`POST "https://live.railsbank.com/v1/customer/cards/{{card_id}}/activate"`

  - A `created` card is cool, but an `activated` card is awesome; it has access to global banking.
  - This simple endpoint will **activate** the card.

## Suspend a Card

  > **Example Request**

  ```shell
    --request POST "https://live.railsbank.com/v1/customer/cards/{{card_id}}/suspend"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```
  `POST "https://live.railsbank.com/v1/customer/cards/{{card_id}}/suspend"`

  - Naturally, there will come a time when you or your endusers need to `suspend` a card. This endpoint allows you to do so in one easy call.
  - Once you suspended a card, it can be reactivated at any time by using `activate` endpoint. However, if you don't reactivate it, it will remain suspended.
  - The API will respond with your suspended `card_id`. Simple.

## Fetching Card PINs
  > **Example Request**

  ```shell
    --request GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}/pin"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```
  > **Example Response**

  ```shell
  {
    "pin": "2477"
  }
  ```
  `GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}/pin"`

  - You may want to fetch the PIN number for chip-and-PIN transactions. All you need is your `card_id` to obtain it.
  - Before you fetch you physical card PIN it is important to note:
  - PINs for new cards are generated at 05:05 each day, as part of generating the xml file for the manufacturer.
  - If the PIN is not set you will receive `"error": "card-pin-not-set"`
  - Unfortunately, we do not allow you to update a card PIN via an API call. To do this your users will have to go to an ATM.


## Get Card Image
  > **Example Request**

  ```shell
    --request GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}/image"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourliveapikey>>"
  ```
  > **Example Response**

  ```shell
  {
    "temp_card_image_url": "http://40.127.00.174:8002/Paymentology/585256_F347DBA257102343AB5132840D231AB3.png"
  }
  ```
`GET "https://live.railsbank.com/v1/customer/cards/{{card_id}}/image"`

  - This endpoint provides a URL which can be used to view the card image.
  - The URL is valid for ten minutes.
