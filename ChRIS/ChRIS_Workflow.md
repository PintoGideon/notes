# Medical Image processing with OpenStack and OpenShift

Medical image procesing research happens in relative isolation. The focus is on the output.
Everyone invents their own platform.

### HPC cluster

The previous version of ChRIS ran on an HPC cluster
A HPC cluster is a collection of many seperate servers called nodes which are connected via a fast interconnect.
All cluster nodes have the same components as a laptop or desktop.:CPU cores, memory and disk space. The difference between a personal computer and a cluster node is quanitity, quality and power of the components.

Openshift and openstack to provide enormous scale to take the image processor and run them on many instances simulataneously as well as using advanced hardware.

### OCI/ Docker

Basically, as long as images follow this specification, it should enable anyone to build tools that will work with the images and have the resulting images be compatible with any compliant runtime

This makes it easy for someone with an existing application, port it into a docker image and then get the portability to run that on any system.

### Openshift.Kubernetes

Provides a scaled job framework for running image processors. This means that it takes an image processor and scales it either from one instance or essentially multiple instances. We are also taking advantage of the resource management capabilities of Openshift which allows allocating priorities to the image processors.

Openshift and Kubernetes do not exactly solve the infrastructure problem.OpenStack is esentially a series of commands known as scripts. Those scripts are bundled into packages called projects
that relay tasks that create cloud environments. In order to create those environments, OpenStack relies on 2 other types of software:

1. Virtualization that creates a layer of virtual resources abstracted from hardware.
2. A base operating system that carries out commands given by OpenStack scripts

Thinks about it like this: OpenStack itself doesn't virtualize resources, but rather uses them to build clouds.
OpenStack also doesn't execute commands, but rather relays them to the base OS. All 3 technologies, OpenStack virtualization, and the base OS- must work together. That interdepenency is why so many OpenStack clouds are deployed using Linux.

### Introduction.

![2019-10-07 (3)](https://user-images.githubusercontent.com/15992276/66349879-63eef580-e949-11e9-9ed3-3a681c153d4a.png)

Medical image processing on the research side can take maybe an hour or two to simulate. If you do this on a GPU in a MOC, this is reduced to seconds.

ChRIS takes in input from the data source , pulls it to itself and pushes it out to the compute environment. At the same time, it queries the ChRIS store and pulls one of the plugins in the compute environment. This computer produces an output which is pulled back into ChRIS which allows the user to interact with the data source both input and output.

### Architecture.

![2019-10-07 (9)](https://user-images.githubusercontent.com/15992276/66349877-63eef580-e949-11e9-903e-1fe02d4b8c00.png)

First Data is sent to OpenShift through the IO handler. Then the data is stored inside of Swift. Swift storage policies enable segmentation of Swift Backend clusters. All operations are available through the swift client.
The Process Manager then gets set in operation and that operation is of the form of some docker image to run. The process manager launches a job which has a number of pods in it.

The InitContainer pulls in data from the swift storage, writing it into some local volume and makes it available for the next container. The next container is the image plugin.
The image plugin takes the data that is written to a local disk from an input directory and write the output to the output directory. The publish container takes the result of the output directory and publishes it back to Swift. Once that's done, the ChRIS frontend is able to calll the I/O handler and display the result to the frontend.

The image processor is packaged as a docker image. In this case, we have built it into a environment that is capable of scale where the job can be scaled into on more than one instance by bringing in a shared storage.

### GPU example - Prostate Segmentation

![2019-10-07 (7)](https://user-images.githubusercontent.com/15992276/66349878-63eef580-e949-11e9-9c3f-1cef2a5c4c22.png)

A graphics processing unit (GPU) is a specialized electronic circuit designed to rapidly manipulate and alter memory to accelerate the creation of images in a frame buffer intended for output to a display device. GPUs are used in embedded systems, mobile phones, personal computers, workstations, and game consoles. Modern GPUs are very efficient at manipulating computer graphics and image processing.

### XTK viewer

XTK suppprts a variety of scientific file formats out of the box.

**_Surface Models/Mesh Files_**

- .STL - Standard Tesselation
- .VTK - The Visualization toolkit polydata
- .FSM, .INFLATED, .SMOOTHWM, .SPHERE, .PIAL , .ORIG- Freesurfer Meshes
- .OBJ- Wavefront .obj format

**_DICOM/VOLUME FILES_**

- .NRRD
- .NII
- .NII.GZ
- .MGH
- .MGZ
- .DICOM, .DCM

### File preview

Volumes are rendered in 2D for file previews

```javascript
r = new X.renderer2D();
r.orientation = "x";
object = new X.volume();
r.init();
r.add(object);
r.camera.position = [0, 400, 0];
r.render();
```

### Todo:

1 - Error handling for the image Viewer
2-

### Cornerstone Viewer

````

```javascript
handleOpenDicomdir = (files) => {
  this.files = [];
  for (let i = 0; i < files.length; i++) {
    this.files.push(files[i]);
  }
  this.setState({ visibleOpenMultipleFilesDlg: true });
};
````

```javascript


  hideOpenMultipleFilesDlg=()=>{
      this.setState({visibleOpenMultipleFilesDlg:false})
      this.openMultiplesFilesCompleted();
  }

  openMultipleFilesCompleted=()=>{
      const sliceMax=this.props.files.length;
      runTool("openImage",0);
      const sliceMax=this.props.files.length;
      this.setState({
          sliceMax:sliceMax
      })
  }

    visibleOpenMultipleFilesDlg && (
        <OpenMultipleFilesDlg
        onClose={this.hideOpenMultipleFilesDlg}
        files={this.files}
        origin={"local}
        />
    )

```

```javascript
class OpenMultipleFilesDlg extends PureComponent {
  componentDidMount() {
    this.files = this.props.files;

    let imageIds = [];

    for (let i = 0; i < files.length; i++) {
      const file = files[i];
      imageIds.push(cornerstoneWADOImageLoader.wadouri.fileManager.add(file));
    }

    for (let i = 0; i < files.length; i++) {
      const file = files[i];
      cornerstone.loadImage(imageIds[i]).then((image) => {
        let item = null;
        item = {
          imageId: imageIds[i],
        };
        this.items.push(item);
        this.count++;
        const progress = Math.floor(this.count * (100 / files.length));
        if (progress > this.nextProgress) {
          this.nextProgress += this.step;
          this.setState({ progress: progress });
        }
        if (this.count === files.length) {
          this.items.sort((l, r) => {
            return (l.instanceNumber = r.instanceNumber);
          });
          this.props.setFilesStore(this.items);
        }
      });
    }
  }
}
```

```javascript
export const filesStore = (files) => {
  return {
    type: FILES_STORE,
    files: files,
  };
};
```

```javascript
const displayImageFromFiles = (index) => {
  const files = this.props.files;
  const image = files[index].image;
  const imageId = files[index].imageId;
  this.fileName = files[index].name;
  this.sliceIndex = index;

  const element = this.dicomImage;
  element.addEventListener("cornerstonenewimage", this.onNewImage);
  element.addEventListener("cornerstoneimagerendered", this.onImageRendered);

  this.image = image;
  this.isDicom = true;

  this.PatientsName = image.data.string("x00100010");
  let stack = { currentImageIdIndex: 0, imageIds: "" };
  this.numberOfFrames = image.data.intString("x00280008");
  if (this.numberOfFrames > 0) {
    let imageIds = [];
    for (var i = 0; i < this.numberOfFrames; ++i) {
      imageIds.push(imageId + "?frame" + i);
    }
    stack.imageIds = imageIds;
  }
  cornerstone.displayImage(element, image);
  this.enableTool();
};

const runTool = (toolName, opt) => {
  switch (toolName) {
    case "openImage": {
      cornerstone.disable(this.dicomImage);
      this.dispayImageFromFiles(opt);
    }
  }
};
```

```javascript
enableTool = (toolName, mouseButtonNumber) => {
  if (this.props.dcmEnableTool) return;
  // Enable all tools we want to use with this element
  const WwwcTool = cornerstoneTools.WwwcTool;
  const LengthTool = cornerstoneTools["LengthTool"];
  const PanTool = cornerstoneTools.PanTool;
  const ZoomTouchPinchTool = cornerstoneTools.ZoomTouchPinchTool;
  const ZoomTool = cornerstoneTools.ZoomTool;
  const ProbeTool = cornerstoneTools.ProbeTool;
  const EllipticalRoiTool = cornerstoneTools.EllipticalRoiTool;
  const RectangleRoiTool = cornerstoneTools.RectangleRoiTool;
  const FreehandRoiTool = cornerstoneTools.FreehandRoiTool;
  const AngleTool = cornerstoneTools.AngleTool;
  const MagnifyTool = cornerstoneTools.MagnifyTool;
  const StackScrollMouseWheelTool = cornerstoneTools.StackScrollMouseWheelTool;

  cornerstoneTools.addTool(MagnifyTool);
  cornerstoneTools.addTool(AngleTool);
  cornerstoneTools.addTool(WwwcTool);
  cornerstoneTools.addTool(LengthTool);
  cornerstoneTools.addTool(PanTool);
  cornerstoneTools.addTool(ZoomTouchPinchTool);
  cornerstoneTools.addTool(ZoomTool);
  cornerstoneTools.addTool(ProbeTool);
  cornerstoneTools.addTool(EllipticalRoiTool);
  cornerstoneTools.addTool(RectangleRoiTool);
  cornerstoneTools.addTool(FreehandRoiTool);
  cornerstoneTools.addTool(StackScrollMouseWheelTool);

  this.props.setDcmEnableToolStore(true);
};
```

### Pipelines

```sh
http -a cubeadmin:cubeadmin1234 POST http://localhost:8010/api/v1/pipelines/ Content-Type:application/vnd.collection+json Accept:application/vnd.collection+json "template":
  {"data":[{"name":"name","value":"Automatic Fetal Brain Reconstruction Pipeline v1.0.0"},
  {"name": "authors", "value": "Jennings Zhang <Jennings.Zhang@childrens.harvard.edu>"},
  {"name": "Category", "value": "MRI"},
  {"name": "description", "value":
  "Automatic fetal brain reconstruction pipeline developed by Kiho's group at the FNNDSC. Features machine-learning based brain masking and quality assessment."},
  {"name":"locked","value":false},
  {"name":"plugin_tree","value":"[
    {\"plugin_name\":\"pl-fetal-brain-mask\"             ,\"plugin_version\":\"1.2.1\"    ,\"previous_index\":null},
    {\"plugin_name\":\"pl-ANTs_N4BiasFieldCorrection\"   ,\"plugin_version\":\"0.2.7.1\"  ,\"previous_index\":0,
      \"plugin_parameter_defaults\":[{\"name\":\"inputPathFilter\",\"default\":\"extracted/0.0/*.nii\"}]},
    {\"plugin_name\":\"pl-fetal-brain-assessment\"       ,\"plugin_version\":\"1.3.0\"   ,\"previous_index\":1},
    {\"plugin_name\":\"pl-irtk-reconstruction\"          ,\"plugin_version\":\"1.0.3\"   ,\"previous_index\":2}
  ]"}]}}

```
```
 return {
    data: {
      name: "Fastsurfer",
      authors: "Gideon Pinto",
      Category: "MRI",
      description: "Fastsurfer Workflow",
      locked: false,
      plugin_tree: [
        {
          plugin_name: "pl-dircopy",
          plugin_version: "",
          previous_index: null,
        },
        {
          plugin_name: "pl-pfdicom_tagExtract",
          plugin_version: "",
          previous_index: 1,
        },
        {
          plugin_name: "pl-pfdicom_tagSub",
          plugin_version: "",
          previous_index: 1,
        },
        {
          plugin_name:"
        },
      ],
    },
  };

```