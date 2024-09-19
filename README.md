
import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    // Combine uploaded file and existing documents into a single array
    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    return (
        <div className={styles.uploadDocuments}>
            <h2>Documents Review</h2>
            {allDocuments.length > 0 ? (
                <div className={styles.reviewSection}>
                    <div className={styles.preview}>
                        {allDocuments.map((doc, index) => (
                            <div key={index} className={styles.previewItem}>
                                {/* Handle uploaded file (Blob object) and existing document (URL string) differently */}
                                {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                    <img
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        alt={doc.name || "Document Preview"}
                                        className={styles.imagePreview}
                                    />
                                ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                    <iframe
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        title={PDF Preview ${index}}
                                        className={styles.pdfPreview}
                                        width="500px"
                                        height="500px"
                                    />
                                ) : doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx') ? (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document (DOCX)
                                    </a>
                                ) : (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document
                                    </a>
                                )}
                            </div>
                        ))}
                    </div>
                </div>
            ) : (
                <p className={styles.noFile}>No document available</p>
            )}
        </div>
    );
};

export default UploadDocuments;
import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    // Combine uploaded file and existing documents into a single array
    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    return (
        <div className={styles.uploadDocuments}>
            <h2>Documents Review</h2>
            {allDocuments.length > 0 ? (
                <div className={styles.reviewSection}>
                    <div className={styles.preview}>
                        {allDocuments.map((doc, index) => (
                            <div key={index} className={styles.previewItem}>
                                {/* Handle uploaded file (Blob object) and existing document (URL string) differently */}
                                {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                    <img
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        alt={doc.name || "Document Preview"}
                                        className={styles.imagePreview}
                                    />
                                ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                    <iframe
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        title={PDF Preview ${index}}
                                        className={styles.pdfPreview}
                                        width="500px"
                                        height="500px"
                                    />
                                ) : doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx') ? (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document (DOCX)
                                    </a>
                                ) : (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document
                                    </a>
                                )}
                            </div>
                        ))}
                    </div>
                </div>
            ) : (
                <p className={styles.noFile}>No document available</p>
            )}
        </div>
    );
};

export default UploadDocuments;


ERROR in ./src/components/Pages/UploadDocuments.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
SyntaxError: /home/ec2-user/environment/Claim-assist-v2/web-app/src/components/Pages/UploadDocuments.js: Unexpected token, expected "}" (34:51)

  32 |                                     <iframe
  33 |                                         src={doc.url ? doc.url : URL.createObjectURL(doc)}
> 34 |                                         title={PDF Preview ${index}}
     |                                                    ^
  35 |                                         className={styles.pdfPreview}
  36 |                                         width="500px"
  37 |                                         height="500px"
    at constructor (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:362:19)
    at FlowParserMixin.raise (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:3260:19)
    at FlowParserMixin.unexpected (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:3280:16)
    at FlowParserMixin.expect (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:3590:12)
    at FlowParserMixin.jsxParseExpressionContainer (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6688:10)
    at FlowParserMixin.jsxParseAttributeValue (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6655:21)
    at FlowParserMixin.jsxParseAttribute (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6704:38)
    at FlowParserMixin.jsxParseOpeningElementAfterName (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6718:28)
    at FlowParserMixin.jsxParseOpeningElementAt (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6713:17)
    at FlowParserMixin.jsxParseElementAt (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6737:33)
    at FlowParserMixin.jsxParseElement (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6800:17)
    at FlowParserMixin.parseExprAtom (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6810:19)
    at FlowParserMixin.parseExprSubscripts (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10584:23)
    at FlowParserMixin.parseUpdate (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10569:21)
    at FlowParserMixin.parseMaybeUnary (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10549:23)
    at FlowParserMixin.parseMaybeUnaryOrPrivate (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10403:61)
    at FlowParserMixin.parseExprOps (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10408:23)
    at FlowParserMixin.parseMaybeConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10385:23)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10348:21)
    at /home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5631:39
    at FlowParserMixin.tryParse (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:3598:20)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5631:18)
    at /home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10318:39
    at FlowParserMixin.allowInAnd (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:11933:12)
    at FlowParserMixin.parseMaybeAssignAllowIn (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10318:17)
    at FlowParserMixin.parseParenAndDistinguishExpression (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:11195:28)
    at FlowParserMixin.parseParenAndDistinguishExpression (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5724:18)
    at FlowParserMixin.parseExprAtom (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10851:23)
    at FlowParserMixin.parseExprAtom (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:6815:20)
    at FlowParserMixin.parseExprSubscripts (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10584:23)
    at FlowParserMixin.parseUpdate (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10569:21)
    at FlowParserMixin.parseMaybeUnary (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10549:23)
    at FlowParserMixin.parseMaybeUnaryOrPrivate (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10403:61)
    at FlowParserMixin.parseExprOps (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10408:23)
    at FlowParserMixin.parseMaybeConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10385:23)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10348:21)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5682:18)
    at /home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10318:39
    at FlowParserMixin.allowInAnd (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:11933:12)
    at FlowParserMixin.parseMaybeAssignAllowIn (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10318:17)
    at FlowParserMixin.tryParseConditionalConsequent (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5156:29)
    at FlowParserMixin.parseConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5117:14)
    at FlowParserMixin.parseMaybeConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10389:17)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10348:21)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5682:18)
    at /home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5151:77
    at FlowParserMixin.forwardNoArrowParamsConversionAt (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5201:16)
    at FlowParserMixin.parseConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:5151:27)
    at FlowParserMixin.parseMaybeConditional (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10389:17)
    at FlowParserMixin.parseMaybeAssign (/home/ec2-user/environment/Claim-assist-v2/web-app/node_modules/@babel/parser/lib/index.js:10348:21)
ERROR
[eslint] 
src/components/Pages/UploadDocuments.js
  Line 34:51:  Parsing error: Unexpected token, expected "}" (34:51)
