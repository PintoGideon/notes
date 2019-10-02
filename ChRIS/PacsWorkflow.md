### Intro to ChRIS

ChRIS is a web-based data storage and data processing workflow manager. ChRIS is suited to medical image data, providing the ability to seamlessly collect data from typical sources found in hospitals (Such as Picture ,Archive, and Communications Systems PACS), import from CDs/DVDs, users, desktops etc. ChRIS not only manages data collection and organization, but it also provides a large library of piplines to analyze imported data. This library is easily extensible via a simple plugin mechanism, and ChRIS also provides the ability to directly interact with compute clusters for data analysis.

Plugins are the mechanisms by which users create feeds, and plugins are available for importing data, performing various analytical processing on data , and also providing detailed visualization of data and results-- all withing the single-context approach of the ChRIS user interface.

The beauty of ChRIS lies in it's ability to integrate to High Performance Clusters (HPC) resoruces, allowing users to process data on powerful clusters without needing to manage data and results or worry about the transfer and management of data between local computers and the remote HPC.



### Workflow to discuss

The PACS plugin does not actually talk to PACS directly. It interacts with an intermediary service , typically 'pfdcm'. This intermediary service actually connects to a PACS and performs queries which it returns to this app.

![pacs](https://user-images.githubusercontent.com/15992276/66057044-92269c80-e527-11e9-87ab-226b895aab8c.png)

The plugin does not need to know specific details on the PACS IP port etc. All of this information is managed by 'pfdcm'. This does mean that pfdcm needs to be instantiated correctly.

We can pass a <jsonstring> to set 'pdfcm' internal lookup and add new PACS entries. It is possible to pass a 'pfdcm' complinat message string using [--msg <jsonMsgString>]. This <jsonstring> can be used to perform a query. The simplest Mechanism of query will be through the --patientID' and 'PACService' flags. The <outputdir> positional argument is Mandatory as it defines the output directory (or relative dir when called through the ChRIS API) for results tables/files. Results from this app are typically three files in the <outputdir>.

![pacs_pull(1)](https://user-images.githubusercontent.com/15992276/66057122-b3878880-e527-11e9-84eb-001b03ba423f.png)


### What's missing in the current ChRIS_UI

![CreateFeed(5)](https://user-images.githubusercontent.com/15992276/66061933-f6e5f500-e52f-11e9-82fe-8e9c8068f843.png)

Currently while creating the Feed, The UI only allows us to import files into ChRIS Storage from our local filesystem.
We want to create a feed browser (or something similar) which allows the user to select files from the swift storage and pull them as part of the local feed. A possible workflow would be perform a search operation on the Swift storage, receive a number of files as our results, choose the files we want to pull to return to the dircopy plugin. Similar workflow to the PACS operation below.

![Workflow](https://user-images.githubusercontent.com/15992276/66072057-d58f0400-e543-11e9-900f-b12b7ea66640.jpg)


As shown in the image above, a hook already exists to search and filter files from the Swift Storage.


![CreatedFeed(7)](https://user-images.githubusercontent.com/15992276/66070684-54cf0880-e541-11e9-95b2-01eb45a52eaf.png)

![CreateFeed(6)](https://user-images.githubusercontent.com/15992276/66070663-4bde3700-e541-11e9-8b9a-af13a4a5818f.png)



