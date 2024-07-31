Here is how you can dynamically load images from S3 folders instead of hardcoding:

1. Store the S3 bucket name and base folder paths in constants:

```
const S3_BUCKET = 'my-bucket'; 

const BASE_PATHS = {
  citizenAdvisor: 'portal-slides/citizenadvisor',
  smartRecruit: 'portal-slides/smart-recruit' 
};
```

2. Create a function to get the full S3 path for an image:

```
function getS3Path(name, folder) {
  return `https://${S3_BUCKET}.s3.amazonaws.com/${BASE_PATHS[folder]}/${name}`;
}
```

3. Import the AWS SDK and create an S3 client:

```
import { S3 } from 'aws-sdk';

const s3 = new S3();
``` 

4. Use `listObjectsV2` to get all objects in a folder:

```
async function getFolderImages(folder) {
  const params = {
    Bucket: S3_BUCKET, 
    Prefix: `${BASE_PATHS[folder]}/`,
  };

  const data = await s3.listObjectsV2(params).promise();

  return data.Contents.map(item => getS3Path(item.Key, folder));
}
```

5. Call it to load images:

```
const descriptionImages = await getFolderImages('citizenAdvisor/description'); 
const flowImages = await getFolderImages('citizenAdvisor/solutionFlows');
```

So in summary, use the SDK to dynamically get folders/files from S3 instead of hardcoded paths.
