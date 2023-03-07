# Cloud Computing Fundamentals
## Course Introduction
Insert courseObjectives.png here

## Module 4: Where do I store this stuff?
### 1. Storage options available in Google Cloud
Every application needs to store data, and requiere different storage database solutions.

#### Storage offerings
Google Cloud offers: relational and non-relational databases and worldwide object storage. It provides managed storage and database services that are scalable and reliable. Including:
- Cloud Storage
- Cloud SQL (Relational DB)
- Cloud Spanner (Relational DB)
- Firestore (NoSQL DB)
- Bigtable (NoSQL DB)
 
 #### Common cloud storage use cases
 - **Content storage and delivery:** Videos, images, etc. It's for files
 - **Data analytics and general compute:** Users can process or expose their data to analytics tools.
 - **Backup and archival storage:** Users can migrate infrequently accessed content to cheaper cloud storage options.
 
 #### For users with databases
 There are two options:
 1. Migrate existing databases to the cloud. Moving from MySQL to CloudSQL.
 2. Innovate, build or rebuild for the cloud.

### 2. Structured and unstructured data storage
#### Untructured data
Unstructured data is information stored in a non-tabular form such as documents, images, and audio files. This is best suited to Cloud Storage. This kind of data is more difficult to process since there is no internal identifier.
It often includes text and multimedia content, for example, email messages, documents, photos, videos, presentations, and web pages.

#### Structured data
It represents information stored in tables, rows, and columns. You can expect this type of data to be organized and clearly defined and usually easy to capture, access, and analyze.
It often includes names, addresses, contact numbers, dates, and billing info. And it also helps to be understood by programming languages and can be manipulated relatively quickly.

#### Choose the right option
Structured data comes in two types: transactional workloads and analytical workloads.
- Transactional workloads stem from Online Transaction Processing systems, which are used when fast data inserts and updates are required to build row-based records
  - Require standardized queries that affect only a few records.
  - **Cloud SQL, Cloud Spanner**
- Analytical workloads stem from Online Analytical Processing systems, which are used when entire datasets need to be read.
  - Complex queries.
  - **BigQuery,  Bigtable**

### 3. Unstructured Storage using Cloud Storage
Cloud Storage is a fully managed scalable service that has a wide variety of uses. Cloud Storage’s primary use is whenever **binary large-object storage** (also known as a **“BLOB”**) is needed.

#### What is Object Storage?
Object storage is a computer data storage architecture that manages data as “objects” and not as a file and folder hierarchy (file storage), or as chunks of a disk (block storage). These objects are stored in a packaged format that contains the binary form of the actual data itself, relevant associated metadata (such as date created, author, resource type, and permissions), and a globally unique identifier.
These unique keys are in the form of URLs, which means object storage interacts well with web technologies. 

#### Cloud Storage classes
- **Standard Storage: ** For frequently accessed data (Hot data).
- **Nearline Storage: ** For storing infrequently accessed data (Once per month).
- **Coldline Storage: ** For reading or modifying data (Once every 90 days).
- **Archive Storage: ** For data archiving, online backup, and dusaster reivery (Once every year)

The order listed goes from the most expensive to the cheapest one. All of them include:
- Unlimited storage.
- Worldwide accesibility and locations.
- Low latency and high durability.
- Uniform experience.
- Geo-redundancy.

#### Cloud Storage Objects
- They are organized into buckets.
- They are inmutable, so that means that when you edit an object, you are creating a new version of it.
- Administrators can either allow each new version to completely overwrite the older one or keep track of each change made to a particular object by enabling “versioning” within a bucket.
- Cloud Storage offers lifecycle management policies for your objects (Delete after *x* days).

### 4. Lab: Cloud Storage: Qwik Start - CLI/SDK


### SQL managed services

### 

### 

### 

### 

### 

### 

### 

### 
