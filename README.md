 return {
            'statusCode': 200,
            'body': json.dumps({
                "answer": llm_answer,
                "source": "N/A"
            })
        }

    # Step 8: Format PDF link and return the LLM answer with source
    pdf_link = format_pdf_link(metadata.get('pdf_link', 'N/A'))
    location_info = f"Page: {metadata.get('page_number', 'N/A')}, Document Link: {pdf_link}"

    # Return the response, ensuring 'page_number' and 'pdf_link' are in the metadata
    return {
        'statusCode': 200,
        'body': json.dumps({
            "answer": llm_answer,
            "source": location_info
        })
    }
