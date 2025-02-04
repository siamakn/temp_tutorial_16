## Create a New Upload

On any page in NOMAD, click **PUBLISH**-> click **Uploads**-> click **CREATE A NEW UPLOAD** -> edit/give name -> click **SAVE**.

<div style="text-align: center;">
    <img src="images/create_upload/create_upload_step_1_2.png" alt="create uploads" width="800">
</div>

<div style="text-align: center;">
    <img src="images/create_upload/create_upload_step_3.png" alt="create uploads" width="800">
</div>

<div style="text-align: center;">
    <img src="images/create_upload/create_upload_step_4.png" alt="create uploads" width="800">
</div>

<div style="text-align: center;">
    <img src="images/create_upload/create_upload_step_5_6.png" alt="create uploads" width="800">
</div>



## Create an Entry Using NOMAD's Built-in Schema:
 Open the upload you just created or [create a new upload.](#create-a-new-upload) -> click **CREATE FROM SCHEMA** -> give a descriptive name, select your desired built-in schema from dropdown or type to see suggestions -> click **CREATE**.

<div style="text-align: center;">
    <img src="images/create_built-in_schema/create_built-in_schema_step_0.png" alt="go to the upload" width="800">
</div>

<div style="text-align: center;">
    <img src="images/create_built-in_schema/create_built-in_schema_step_1.png" alt="create built-in schema" width="800">
</div>

<div style="text-align: center;">
    <img src="images/create_built-in_schema/create_built-in_schema_step_2_3_4.png" alt="create built-in schema" width="800">
</div>



### Built-in Schema for Substances: *Substance ELN*

Now, let's create an entry using the built-in *Substance ELN* schema for **P3HT powder**. Follow the steps for [creating an entry using NOMAD's Built-in Schema.](#create-an-entry-using-nomads-built-in-schema) and select *Substance ELN* in step 3.


??? example "Example: Creating an entry for P3HT powder (screenshots)"

    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_substance_eln_step_1.png" alt="create built-in schema" width="800">
    </div>

    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_substance_eln_step_2_3_4.png" alt="create built-in schema" width="800">
    </div>

??? info "The input data fields that the built-in schema *Substance ELN* offer:"
    The built-in schema *Substance ELN*  provides the following fields for input:
    
    - **substance name:** Automatically used as the entry file name.
    - **datetime:** Allows entry of a date/time stamp.
    - **substance ID:** A unique, human-readable ID for the substance.
    - **detailed substance description:** A free text field for additional information.

    Additional subsections available in the *data* subsection include:
    
    - **elemental composition:** Define the chemical composition with atomic and mass fractions.
    - **pure substance:** Specify if the material is a pure substance purchased from an external vendor, with fields like:
        - Substance name
        - IUPAC name
        - Molecular formula
        - CAS number
        - Inchi Key, SMILES, and more.
    - **substance identifier:** Add identifiers for specific substances.

??? task "Task: Create an ELN entry for a substance (ca. 3 minutes)"
    Create an ELN entry in NOMAD for one of the following substances:
    
    - Chloroform  
    - Glass substrate  
    - Pre-patterned ITO substrates  

    Use the *Substance ELN* schema and include as many details as you like (e.g., Substance Name, Datetime, Substance ID, Description).  

    > **Tip:** Follow the [steps for creating an entry](#create-an-entry-using-nomads-built-in-schema).

### Built-in Schema for Samples: *Generic Sample ELN*

Now, let's create an entry using the built-in *Generic Sample ELN* schema for **P3HT Thin Film**. Follow the steps for [creating an entry using NOMAD's Built-in Schema.](#create-an-entry-using-nomads-built-in-schema) and select *Generic Sample ELN* in step 3.

??? example "Example: Creating an entry for P3HT thin film on glass (screenshots)"
    
    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_sample_eln_step_1.png" alt="create built-in schema for sample" width="800">
    </div>

    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_sample_eln_step_2_3_4.png" alt="create built-in schema for sample" width="800">
    </div>

??? info "The input data fields that the built-in schema *Generic Sample ELN* offers:"
    The built-in schema *Generic Sample ELN* provides the following fields for input:
    
    - **name:** Automatically used as the entry file name.
    - **datetime:** Allows entry of a date/time stamp.
    - **ID:** A unique, human-readable ID for the sample.
    - **description:** A free text field for additional information.

    Additional subsections available in the *data* subsection include:
    
    - **elemental composition:** Define the chemical composition with atomic and mass fractions.
    - **components:** Specify the components used to create the sample, including raw materials or system components.
    - **sample identifier:** Add unique identifiers for the sample.

??? task "Task: Create an ELN entry for a sample (ca. 1 minute)"
    Create an ELN entry in NOMAD for one of the following samples:
    
    - P3HT Thin Film  
    - P3HT Solution  

    Use the *Generic Sample ELN* schema and include as many details as you like (e.g., Short Name, Datetime, ID, Description).  

    > **Tip:** Follow the [steps for creating an entry](#create-an-entry-using-nomads-built-in-schema).

### Built-in Schema for Instruments: *Instrument ELN*

Now, let's create an entry using the built-in *Instrument ELN* schema for **UV Ozone Cleaner**. Follow the steps for [creating an entry using NOMAD's Built-in Schema.](#create-an-entry-using-nomads-built-in-schema) and select *Instrument ELN* in step 3.

??? example "Example: Creating an entry for UV Ozone Cleaner (screenshots)"

    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_instrument_eln_step_1.png" alt="create built-in schema for instrument" width="800">
    </div>

    <div style="text-align: center;">
        <img src="images/create_built-in_schema/create_instrument_eln_step_2_3_4.png" alt="create built-in schema for instrument" width="800">
    </div>

??? info "The input data fields that the built-in schema *Instrument ELN* offers:"
    The built-in schema *Instrument ELN* provides the following fields for input:
    
    - **name:** Automatically used as the entry file name.
    - **datetime:** Allows entry of a date/time stamp.
    - **ID:** A unique, human-readable ID for the instrument.
    - **description:** A free text field for additional information.

    Additional subsections available in the *data* subsection include:
    
    - **instrument identifiers:** Specify the type of instrument and additional metadata, if applicable.

??? task "Task: Create an ELN entry for an instrument (ca. 1 minute)"
    Create an ELN entry in NOMAD for one of the following instruments:
    
    - Ultrasonic Bath  
    - Scale  
    - Optical Spectrometer  
    - Conductivity Board  
    - Spin Coater  

    Use the *Instrument ELN* schema and include as many details as you like (e.g., Short Name, Datetime, ID, Description).  

    > **Tip:** Follow the [steps for creating an entry](#create-an-entry-using-nomads-built-in-schema).