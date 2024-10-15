import React, { useState } from 'react';

// Sample data from your JSON
const claimData = {
  claimassist_history_lambda: {
    approver1_focuses_on: [
      "reason for lost policy",
      "date of loss",
      "police report or official documentation to support lost policy claim",
      "details about circumstances of policy loss",
      "copy of government issued photo ID"
    ],
    approver2_focuses_on: [
      "wet signature on form",
      "matching signatures",
      "specification of date of loss",
      "confirmation of bank account ownership",
      "clarity in circumstances of policy loss",
      "matching account details"
    ],
    additional_information: [
      "Reason for lost policy with details",
      "Date of loss",
      "Police report for lost policy",
      "Details on circumstances of policy loss",
      "Photo identification of policyholder"
    ],
    suggested_action_items: [
      "Resubmit form with wet signature",
      "Resign form if signature mismatch",
      "Provide date of loss",
      "Confirm bank account details",
      "Submit additional supporting documents"
    ],
    detailed_summary: "The verification officers focus on different details to process the claims...",
  },
  claimassist_docextract_lambda: {
    input_payload: {
      filename: "claimassist/claimforms/CL17254323/1.Filled_L2065777_PIF_and_LPF.pdf",
      claimid: "CL17254323"
    },
    output_response: {
      extracted_data: {
        payment_instruction_form: {
          statement_date: "12/06/2024",
          policy_number: "L2065777",
          policy_on_the_life_of: "Mr JC Mcglynn",
          policy_owner: "Mr JC Mcglynn"
        },
        payment_details: {
          bank_name_and_address: "BARCLAYS BANK",
          account_holders_name: "J.C. MC Ghywon",
          account_number: "50614866",
          bank_sort_code: "20-57-40",
          signed_full_name: "Mr Mcglynn, James Christopher",
          signed_date: "13/06/2024"
        },
        lost_policy_form: {
          statement_date: "12/06/2024",
          policy_number: "L2065777",
          policy_on_the_life_of: "Mr J C Mcglynn",
          policy_owner: "Mr JC Mcglynn"
        }
      }
    }
  }
};

const NewFormDisplay
