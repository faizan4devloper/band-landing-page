{
  "claimassist-history-lambda": {
    "Approver1_Focuses_On": [
      "reason for lost policy",
      "date of loss",
      "police report or official documentation to support lost policy claim",
      "details about circumstances of policy loss",
      "copy of government issued photo ID"
    ],
    "Approver2_Focuses_On": [
      "wet signature on form",
      "matching signatures",
      "specification of date of loss",
      "confirmation of bank account ownership",
      "clarity in circumstances of policy loss",
      "matching account details"
    ],
    "Additional_Information": [
      "Reason for lost policy with details",
      "Date of loss",
      "Police report for lost policy",
      "Details on circumstances of policy loss",
      "Photo identification of policyholder"
    ],
    "Suggested_Action_Items": [
      "Resubmit form with wet signature",
      "Resign form if signature mismatch",
      "Provide date of loss",
      "Confirm bank account details",
      "Submit additional supporting documents"
    ],
    "Detailed_Summary": "The verification officers focus on different details to process the claims. Approver1 focuses more on reasons, dates, and documents related to lost policies, while Approver2 focuses on details related to signatures, dates, and account details in forms. Additional details on reasons, dates, documents, and identities would help process similar claims along with suggested actions to resubmit forms with corrections."
  },
  "claimassist-docextract-lambda": {
    "Input_Payload": {
      "filename": "claimassist%2Fclaimforms%2FCL17254323%2F1.Filled_L2065777+PIF+and+LPF.pdf",
      "claimid": "CL17254323"
    },
    "Output_Response": {
      "ExtractedData": {
        "PAYMENT_INSTRUCTION_FORM": {
          "STATEMENT_DATE": "12/06/2024",
          "POLICY_NUMBER": "L2065777",
          "POLICY_ON_THE_LIFE_OF": "Mr JC Mcglynn",
          "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "PAYMENT_DETAILS": {
          "BANK_NAME_AND_ADDRESS": "BARCLAYS BANK",
          "ACCOUNT_HOLDERS_NAME": "J.C.MC Ghywon",
          "ACCOUNT_NUMBER": "50614866",
          "BANK_SORT_CODE": "20-57-40",
          "SIGNED_FULL_NAME": "Mr Mcglynn, James Christopher",
          "SIGNED_DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM": {
          "STATEMENT_DATE": "12/06/2024",
          "POLICY_NUMBER": "L2065777",
          "POLICY_ON_THE_LIFE_OF": "Mr J C Mcglynn",
          "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "LOST_POLICY_FORM_SIGNED": {
          "FULL_NAME": "Mr Mcglynn, James Christopher",
          "DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM_WITNESSED_BY": {
          "FULL_NAME_OF_WITNESS": "SOPHIE PASSFIELD",
          "DATE": "13/06/2024",
          "ADDRESS_OF_WITNESS": "53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES MK18 8PG",
          "OFFICIAL_STAMP": "",
          "DAY_TIME_TELEPHONE_NUMBER_OF_WITNESS": "07732883 700",
          "OCCUPATION_OF_WITNESS": "Teacher"
        }
      },
      "rawtext": [
        "claimassist/claimforms/CL17254323/1: 'Retirement, Investments, Insurance, AVIVA... etc.'"
      ],
      "keyvaluesText": [
        "claimassist/claimforms/CL17254323/1: ['Key: Account holder's name, Value: J.C.MC Ghywon', 'Key: Policy Number:, Value: L2065777', ...etc.]"
      ]
    }
  }
}
