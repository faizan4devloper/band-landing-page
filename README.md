elif tasktype == 'FETCH_EMAIL': # for template based email, tied to generate email of UI
            # write code to fetch single database record based on id 
            claimid = body.get('claimid')
            emailbody=fetchtmpltemail(claimid)
            print('Inside FETCH EMAIL condition')
            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps({
                    'emailbody': emailbody
                    
                })
            }     
