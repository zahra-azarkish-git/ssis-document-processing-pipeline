**SSIS Document Processing Pipeline**

An ETL pipeline built with SQL Server Integration Services (SSIS) and SQL Server to automate document file processing, validate document metadata, manage revision history, and register files in a relational database.

**Overview**

This project demonstrates an automated document processing workflow that integrates file system operations with SQL Server metadata. It is designed to process document files, identify the latest revisions, validate document information, and register eligible files in the database.

The pipeline combines SSIS control flow, T-SQL stored procedures, and database validation logic to reduce manual processing and improve consistency.

**Business Problem**

Processing large volumes of document files manually can lead to duplicate registrations, incorrect revision handling, and inconsistencies between physical files and database records.

This pipeline addresses these challenges by automating file validation, revision checks, database registration, and file management.

**Key Features**

- Automated file processing using SSIS.
- Document identification and metadata validation.
- Latest revision detection using SQL Server window functions.
- Database-driven validation of document revisions.
- Stored procedure-based file registration.
- File ID generation and database integration.
- Separation of eligible files from outdated or ineligible revisions.
- Structured processing workflow for improved maintainability.

**Architecture**

```
    Document Files
        |
        v
    SSIS Control Flow
        |
        v
    File Identification
        |
        v
    SQL Server Validation
        |
        v
    Revision Verification
        |
        v
    Latest Revision Check
        |
    +----------------------+
    |                      |
    v                      v
Eligible Files       Outdated Revisions
        |
        v
    Database Registration
        |
        v
    File ID Generation
        |
        v
    File Management / Archiving
```

_Note: This diagram represents the logical workflow. The exact execution sequence depends on the implemented SSIS packages._

**Technology Stack**

<div class="joplin-table-wrapper"><table><tbody><tr><th><p><strong>Technology</strong></p></th><th><p><strong>Purpose</strong></p></th></tr><tr><td><p>SQL Server</p></td><td><p>Relational data storage and metadata management</p></td></tr><tr><td><p>T-SQL</p></td><td><p>Validation queries and database logic</p></td></tr><tr><td><p>SSIS</p></td><td><p>ETL orchestration and workflow automation</p></td></tr><tr><td><p>Stored Procedures</p></td><td><p>Reusable validation and registration logic</p></td></tr><tr><td><pre><code>ROW_NUMBER()</code></pre></td><td><p>Revision ranking and latest-version identification</p></td></tr><tr><td><p>File System Tasks</p></td><td><p>File movement and directory operations</p></td></tr></tbody></table></div>

**Processing Workflow**

**1\. File Identification**

The pipeline processes input files and identifies document codes using the configured naming convention.

**2\. Metadata Validation**

Document information is checked against SQL Server records to determine whether the file corresponds to a known document.

**3\. Revision Management**

Document revisions are evaluated using database metadata. The ROW_NUMBER() window function can rank revisions within each document group, enabling the identification of the latest revision according to the configured business rules.

**4\. File Registration**

Eligible files are registered in the database through stored procedures. The registration process associates file information with the relevant database records and generates or retrieves the corresponding file identifier.

**5\. File Management**

Files are handled according to validation and revision results. Outdated or ineligible files can be separated from the active processing workflow.

**Database Components**

The SQL Server layer supports the pipeline through:

- Document metadata validation.
- Revision comparison and ranking.
- Stored procedures for file validation and registration.
- File identifier management.
- Queries for verifying processing results.

Database scripts included in this repository should use sample schemas and anonymized data.

**Repository Structure**

```
ssis-document-processing-pipeline/
├── README.md
├── .gitignore
├── LICENSE
├── ssis/
│   └── DocumentProcessing/
├── sql/
│   ├── tables/
│   ├── stored-procedures/
│   └── queries/
├── docs/
│   └── architecture.md
└── sample-data/
    └── README.md
```

**Configuration**

Before running the pipeline, configure the required SQL Server connection managers, source directories, destination directories, and database objects.

Use environment-specific configuration values rather than hardcoding connection details in packages or scripts.

Actual server names, credentials, internal file paths, and production database details must not be committed to a public repository.

**Execution**

1. Open the SSIS solution in a compatible version of Visual Studio with the required SSIS extension installed.
2. Configure the SQL Server connection managers.
3. Create the required database objects using the provided SQL scripts.
4. Configure input and output directories.
5. Run the package in a controlled test environment.
6. Validate database registration results and file-processing outcomes.

The required SQL Server and SSIS versions depend on the project configuration.

**Validation and Testing**

The pipeline should be tested against scenarios such as:

- A valid document with the latest revision.
- A document with multiple revisions.
- An outdated revision.
- A file with no matching database record.
- A duplicate registration attempt.
- A missing or inaccessible input file.
- A database validation or registration failure.

**Security Considerations**

- Do not commit production credentials or connection strings.
- Do not publish confidential company data or actual document files.
- Use anonymized sample records and generic file paths.
- Exclude temporary files, build artifacts, and sensitive configuration from version control.

**Future Improvements**

- Centralized logging and error tracking.
- Retry handling for transient failures.
- Automated execution through SQL Server Agent.
- Processing metrics and operational monitoring.
- Automated deployment and environment-specific configuration.

**License**

This project is distributed under the license specified in the LICENSE file.
