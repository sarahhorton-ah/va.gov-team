## Instructions
This guide is for taking a confirmation number that a user will see have seen on VA.gov and retrieving the form submission PDF / status from the Lighthouse Benefits Intake API

### Getting the `benefits_intake_uuid` via DataDog
1. Navigate to [this](https://vagov.ddog-gov.com/dashboard/s5j-ddd-kb8/benefits---income-and-assets---0969---benefits-intake?tile_focus=3106620964428565) DataDog dashboard widget:  
**NOTE:** Make sure the `env` for the dashboard and the time frame are set correctly (staging -> `eks-staging`, production -> `eks-prod`)

|<img width="720" alt="DataDog Lighthouse submission widget" src="https://github.com/user-attachments/assets/6ef35019-6932-4eba-84de-5085121abb3b" />
|-

|<img width="440" alt="Environment selection dropdown" src="https://github.com/user-attachments/assets/d1dcec17-dc1d-4dfc-b45f-045618e50869" />
|-

|<img width="440" alt="Date-range selection dropdown" src="https://github.com/user-attachments/assets/8bec3ce1-b19e-4637-ae87-259d3593e902" />
|-

2. You should see a list of logs similar to the image above. The second column (outlined in **RED**) should contain a UUID that matches the confirmation number that the user would have seen on the confirmation page on VA.gov. We are interested in the `benefits_intake_uuid` (outlined in **BLUE**) as that will allow us to retrieve the form submission PDF / status from Lighthouse
   
|<img width="720" alt="Lighthouse submission logs" src="https://github.com/user-attachments/assets/651852d0-3c04-40f1-8cfc-ec26a8046659" />
|-

### Retrieving submission information from Lighthouse Benefits Intake
**NOTE:** You will need an API key for the Lighthouse Benefits Intake API. If you don't have one already, you can get a new one [here](https://developer.va.gov/explore/api/benefits-intake/sandbox-access)

#### Current host mapping (as of 07/25/2025)  
**NOTE:** This may be out of date, so if you get a corrupted ZIP back, it's possible the host needs to be updated
- VA.gov Staging: `sandbox-api.va.gov`
- VA.gov Production: `api.va.gov`

#### Retrieving the form PDF
1. Open a terminal
2. Enter the following command (make sure to replace the variables wrapped in `{` with the correct values):  
**NOTE:** Make sure that the host (eg. `sandbox-api.va.gov`) is correct and maps to the correct one for the environment in VA.gov you are interested in

```sh
# benefits_intake_uuid: The UUID we got from DataDog column 3 (outlined in BLUE in the screenshot above)
# apikey: Your API key for use with the Lighthouse Benefits Intake API
# filename: This is the name that the ZIP will assume when it's saved to your computer
curl -X GET 'https://sandbox-api.va.gov/services/vba_documents/v1/uploads/{benefits_intake_uuid}/download' \
  --header 'apikey: {apikey}' \
  --header 'accept: application/zip' \
  > {filename}.zip
```

#### Retrieving the current status of the submission
1. Open a terminal
2. Enter the following command (make sure to replace the variables wrapped in `{` with the correct values):  
**NOTE:** Make sure that the host (eg. `sandbox-api.va.gov`) is correct and maps to the correct one for the environment in VA.gov you are interested in

```sh
# benefits_intake_uuid: The UUID we got from DataDog column 3 (outlined in BLUE in the screenshot above)
# apikey: Your API key for use with the Lighthouse Benefits Intake API
curl -X GET 'https://sandbox-api.va.gov/services/vba_documents/v1/uploads/{benefits_intake_uuid}' \
  --header 'apikey: {apikey}' \
  --header 'accept: application/json'
```

