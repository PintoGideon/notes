### Create Analysis Section


CreateAnalysis -----> PatientLookup.

```typescript

const initialAnalysisState={
    patientID:'',
    currentSelectionID:'',
    selectedStudyUIDs:{}
}

```
Submit an MRN ID

```javascript
const dcmImages= await pacs_integration.queryPatientFiles(patiendId);
// OR
const dcmImages= await chris_integration.fetchPacsFiles(patiendId);


async function fetchPacsFiles(patientId) {
    const res = await client.getPacsFiles({
        PatientID:patientID,
        limit:1000
    })
}

```

The covidnet script uses a python module named dcmread to push files into swift storage. All the meta info is read and dumped into the pacs api endpoint.

### Study instances

Grouped by study instances


```javascript

const studyInstance[];

dcmImages.forEach((img)=>{
    studyInstances.push({
        studyInstanceUID: img.StudyInstanceUID,
        studyDescription:img.StudyDescription,
        modality:img.modality,
        studyDate:formatDate(img.StudyDate)
    })
})


```


### Create Analysis Detail Page


1. It has the Patient Information
2. It has filter component
3. Selection Sudy
4. If you select a study ID, add it to a list



```javascript

dispatch({
    type:actionType,
    payload:{
        studyUID:img.StudyInstanceUID,
    }
})


<input type='checkbox' checked={isSelected} onChange={()=>addImgToAnalysis(isSelected, img)}/>

```


### Past Analysis Table

Figure out how covidnet_ui creates a feed.



```javascript

Configure ChRIS to use the correct XRay models.


export const PluginModels : Plugins = {
    XRayModels : {
        'COVID-Net':'pl-covidnet'
    },
    CTModels : {
        'CT-COVID-Net': 'pl-ct-covidnet'
    }
}


// Create a Feed using Dircopy

const data={"dir":image.fname, title:img.PatientID};

// Add Med2Img to the pipelne

// Add pl-covidnet

const pluginNeeded= img.Modality ==='CR' ? XRayModel:CTModel;


```


