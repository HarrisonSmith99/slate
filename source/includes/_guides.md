# Guides
## Using these Guides
  - We want you to explore (and give feedback to) Railsbank. The following guides will take you on a step-by-step tour through the most important aspects of the API.
  - In the following examples, we will be using the **PlayLive** environment. To use these examples in a **Play** or **Live** environment, simply change the Authorization `API-Key` to the correct Key and change the base URL to `https://play.railsbank.com` or `https://live.railsbank.com` respectively.
  - We aim to be simple and self-service; anyone can make their way through any of these guides.
## What you will need
  - Playlive API Keys
  - Access to a terminal or a testing tool like Postman.

# Send Money to a Beneficiary
  - The fundamental feature of almost any financial services business is sending – and therefore also receiving money.
  - In this short guide we will demonstrate how easy it is with Railsbank.
  - There are five steps:


      1. Onboard an Enduser
      2. Give the Enduser a Ledger
      3. Fetch the Ledger (in order to fund the ledger)
      4. Add a new Beneficiary
      5. Send Money to the Beneficiary




## Onboard an Enduser
  > **Example Request**

  ```shell
      --request POST "https://playlive.railsbank.com/v1/customer/endusers"
      --header "Content-Type: application/json"
    	--header "Accept: application/json"
    	--header "Authorization: API-Key <<yourAPI-Key>>"
      {
        "person": {
             "name": "John Smith",
             "email": "johnsmith@gmail.com",
             "date_of_birth": "1970-11-05",
             "address": {
               "address_refinement": "Apartment 42",
               "address_number": "29",
               "address_street": "Acacia Road",
               "address_city": "London",
               "address_postal_code": "FX20 7XS",
               "address_iso_country": "GBR"
             },
             "nationality": [
               "British"
             ],
             "country_of_residence": [
               "GBR"
             ]
        }
      }
  ```
  > **Example Curl Request**

  ```shell

    curl -X POST "https://playlive.railsbank.com/v1/customer/endusers" -H "accept: application/json" -H "Authorization: API-Key <<yourAPI-Key>>" -H "Content-Type: application/json" -d "{ \"person\": { \"name\": \"John Smith\", \"email\": \"johnsmith@gmail.com\", \"date_of_birth\": \"1970-11-05\", \"address\": { \"address_refinement\": \"Apartment 42\", \"address_number\": \"29\", \"address_street\": \"Acacia Road\", \"address_city\": \"London\", \"address_postal_code\": \"FX20 7XS\", \"address_iso_country\": \"GBR\" }, \"nationality\": [ \"British\" ], \"country_of_residence\": [ \"GBR\" ]} }"

  ```
  > **Example Response**

  ```shell
    {
      "enduser_id": "5cf0e7ed-439e-4781-b42c-8d0c0c15adf3"
    }
  ```

  - Endusers are the customers of our customer.
  - In this case the enduser will be a person – John Smith – who will 'hold' the ledger that we will send money from, and the beneficiary that we will send money too (meaning the beneficiary is essentially in their list of recipients).
  - This is the **minimum data** required to create an enduser that can hold a ledger and a beneficiary.
  - The API will respond with an `enduser_id`, which you need to save as a variable because we're going to use it to create a ledger and a beneficiary.

## Give the Enduser a Ledger
  > **Example Request**

  ```shell
      --request POST "https://playlive.railsbank.com/v1/customer/ledgers"
      --header "Content-Type: application/json"
      --header "Accept: application/json"
      --header "Authorization: API-Key <<yourAPI-Key>>"
      {
        "holder_id": "{{ENDUSER_ID}}",
        "partner_product": "PayrNet-GBP-1",
        "asset_class": "currency",
        "asset_type": "gbp",
        "ledger_type": "ledger-type-single-user",
        "ledger_who_owns_assets": "ledger-assets-owned-by-me",
        "ledger_primary_use_types": ["ledger-primary-use-types-payments"],
        "ledger_t_and_cs_country_of_jurisdiction": "GB"
      }
  ```
  > **Example Curl Request**

  ```shell
      curl -X POST "https://playlive.railsbank.com/v1/customer/ledgers" -H "accept: application/json" -H "Authorization: API-Key <<yourAPi-Key>>" -H "Content-Type: application/json" -d "{ \"holder_id\": \"{{ENDUSER_ID}}\", \"partner_product\": \"PayrNet-GBP-1\", \"asset_class\": \"currency\", \"asset_type\": \"gbp\", \"ledger_type\": \"ledger-type-single-user\", \"ledger_who_owns_assets\": \"ledger-assets-owned-by-me\", \"ledger_primary_use_types\": [\"ledger-primary-use-types-payments\"], \"ledger_t_and_cs_country_of_jurisdiction\": \"GB\"}"
  ```
  > **Example Response**

  ```shell
      {
        "ledger_id": "5cf0eb53-085e-4b1a-ab8d-ecd6c3b9bbd1"
      }
  ```
  - Ledgers are digital bank accounts. In the Playlive environment they are live bank accounts, meaning you can send and receive money with them.
  - In this example we will create a GBP ledger, which is connected to UK Faster Payments, meaning when we fund it the money will be received within minutes.
  - Playlive has strict compliance limits. Most importantly, the maximum amount for transactions is £5 and the maximum cumulative amount for transactions is £20 – It is designed for this sort of simple, early testing of the API. Real testing of your integration with Railsbank can be done in the Play environment with virtual money.
  - The API will respond with a `ledger_id`, which you need to save as a variable because we're going to use it to fetch the ledger to get the account details.

## Fetch the ledger
  > **Example Request**

  ```shell
    --request GET "https://playlive.railsbank.com/v1/customer/ledgers/{{LEDGER_ID}}"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourAPI-Key>>"
  ```
  > **Example Curl Request**

  ```shell
      curl -X GET "https://playlive.railsbank.com/v1/customer/ledgers/{{LEDGER_ID}}" -H "accept: application/json" -H "Authorization: API-Key <<yourAPI-Key>>"
  ```
  > **Example Response**

  ```shell
      {
        "iban": "GB26PAYR040052020323XX",
        "last_modified_at": "2019-05-31T08:52:35.114Z",
        "ledger_primary_use_types": [
          "ledger-primary-use-types-payments"
        ],
        "ledger_id": "5cf0eb53-085e-4b1a-ab8d-ecd6c3b9bbd1",
        "ledger_holder": {
          "enduser_id": "5cf0e7ed-439e-4781-b42c-8d0c0c15adf3",
          "person": {
            "name": "John Smith"
          }
        },
        "ledger_who_owns_assets": "ledger-assets-owned-by-me",
        "uk_sort_code": "040052",
        "partner_ref": "payrnet",
        "holder_id": "5cf0e7ed-439e-4781-b42c-8d0c0c15adf3",
        "partner_id": "58fe2ce1-b90b-4868-b40d-cb3bc43395d9",
        "ledger_t_and_cs_country_of_jurisdiction": "GB",
        "bic_swift": "PAYRGB2LXXX",
        "ledger_status": "ledger-status-ok",
        "amount": 0,
        "created_at": "2019-05-31T08:52:35.114Z",
        "partner_product": "PayrNet-GBP-1",
        "partner": {
          "partner_id": "58fe2ce1-b90b-4868-b40d-cb3bc43395d9",
          "company": {
            "name": "PayrNet"
          },
          "partner_ref": "payrnet"
        },
        "ledger_iban_status": "ledger-iban-status-ok",
        "asset_type": "gbp",
        "uk_account_number": "020323XX",
        "asset_class": "currency",
        "ledger_type": "ledger-type-single-user"
    }
  ```

  - In order to send money to a beneficiary we need to have money on our ledger. We therefore need to fund the ledger from a bank account outside of Railsbank. The easiest way to do this is to send money via the UK Faster Payments scheme. If your bank account does not have access to UKFP, please [contact us](mailto:support@railsbank.com) and we will help you fund your ledger.
  - In order to fund the ledger, we need to know the account details. To do that we need to fetch the ledger.
  - Once you have the account details, simply send any amount less than £5 to the ledger from your bank account.
  - Once you have sent the money, keep fetching the ledger (if you want to use a webhook see the webhook endpoints below) until the `amount` field changes to the amount you sent.
  - The API will respond with all the information about the ledger, like so. Note the `uk_account_number` and `uk_sort_code`.

## Add a new Beneficiary
  > **Example Request**

  ```shell
     --request POST "https://playlive.railsbank.com/v1/customer/beneficiaries"
     --header "Content-Type: application/json"
     --header "Accept: application/json"
     --header "Authorization: API-Key <<yourAPI-Key>>"
     {
       "asset_class": "currency",
       "asset_type": "gbp",
       "default_account": {
         "account_number": "45564658",
         "bank_country": "GB",
         "bank_code": "040004",
         "bank_name": "Monzo",
         "asset_type": "gbp",
         "asset_class": "currency",
         "bank_code_type": "sort-code"
       },
       "holder_id": "{{ENDUSER_ID}}",
       "person": {
            "name": "John Smith",
            "email": "johnsmith@gmail.com",
            "date_of_birth": "1970-11-05",
            "address": {
              "address_refinement": "Apartment 42",
              "address_number": "29",
              "address_street": "Acacia Road",
              "address_city": "London",
              "address_postal_code": "FX20 7XS",
              "address_iso_country": "GBR"
            },
            "nationality": [
              "British"
            ],
            "country_of_residence": [
              "GBR"
            ]
       }   
     }
  ```
  > **Example Curl Request**

  ```shell
  curl -X POST "https://playlive.railsbank.com/v1/customer/beneficiaries" -H "accept: application/json" -H "Authorization: API-Key y6jsxhvf507460eobh03bjjyi68termc#de3vkuuw7ao9u35lrivhs4cpp3ijum5jodrya7xg2nq5k0a9c9drbobz6bsm4che" -H "Content-Type: application/json" -d "{ \"uk_account_number\": \"55631106\", \"uk_sort_code\": \"040004\", \"asset_class\": \"currency\", \"asset_type\": \"gbp\", \"holder_id\": \"5cf0e7ed-439e-4781-b42c-8d0c0c15adf3\", \"person\": { \"name\": \"John Smith\", \"email\": \"johnsmith@gmail.com\", \"date_of_birth\": \"1970-11-05\", \"address\": { \"address_refinement\": \"Apartment 42\", \"address_number\": \"29\", \"address_street\": \"Acacia Road\", \"address_city\": \"London\", \"address_postal_code\": \"FX20 7XS\", \"address_iso_country\": \"GBR\" }, \"nationality\": [ \"British\" ], \"country_of_residence\": [ \"GBR\" ] } }"
  ```
  > **Example Response**

  ```shell
    {
      "beneficiary_id": "bb8b2428-f94c-41df-8e82-a895ab4d6ac8"
    }
  ```
  - A beneficiary is just an entity that receives money from Railsbank ledgers.
  - The enduser who is sending the money to the beneficiary is the `holder` of that beneficiary.
  - In the example to the right we are using the minimum required data for creating a beneficiary.
  - In this case, use whatever GBP account you like for the beneficiary account. Why not just send your money back to your account?
  - The API will respond with a `beneficiary_id`, which you need to save as a variable because we're going to use it to send money to the beneficiary.

## Send money to the Beneficiary
  > **Example Request**

  ```shell
    --request POST "https://playlive.railsbank.com/v1/customer/transactions"
    --header "Content-Type: application/json"
    --header "Accept: application/json"
    --header "Authorization: API-Key <<yourAPI-Key>>"
    {
        "payment_type": "payment-type-UK-FasterPayments",
        "reference": "this is a test payment",
        "amount": "1",
        "ledger_from_id": "{{LEDGER_ID}}",
        "beneficiary_id": "{{BENEFICIARY_ID}}",
    }
  ```
  > **Example Curl Request**

  ```shell
      curl -X POST "https://playlive.railsbank.com/v1/customer/transactions" -H "accept: application/json" -H "Authorization: API-Key y6jsxhvf507460eobh03bjjyi68termc#de3vkuuw7ao9u35lrivhs4cpp3ijum5jodrya7xg2nq5k0a9c9drbobz6bsm4che" -H "Content-Type: application/json" -d "{ \"payment_type\": \"payment-type-UK-FasterPayments\", \"reference\": \"this is a test payment\", \"amount\": \"1\", \"ledger_from_id\": \"{{LEDGER_ID}}\", \"beneficiary_id\": \"{{BENEFICIARY_ID}}\",}"
  ```
  > **Example Response**

  ```shell
    {
      "transaction_id": "bb8b2428-f94c-41df-8e82-a895ab4d782"
    }
  ```
  - If the money has arrived in your ledger and you have created your beneficiary you have everything you need to send money to the beneficiary.
  - Once again the funds will travel via the UK Faster Payments scheme, arriving in the beneficiary account within minutes.
  - The API will respond with a `transaction_id` and the beneficiary should receive the money.
  - If you want to check the transaction, use the `transaction_id` and check out the Fetch a transaction endpoint.
