It is very common to export measurement data into a tabular format such as `.csv` or `.xlsx`.Following, we'll explore how to utilize NOMAD's **tabular parser** effectively to enhance our data documentation and visualization.

Our objectives are:
* 1. To upload our `.csv` or `.xlsx` data files onto NOMAD as **entries**, visualize them, publish and get a DOI.
* 2. Enhance our custom schema by making NOMAD parse the data within these files, and then visualize the data in plots that can be viewed in our customized ELN. 

To achieve this, we will write a schema using the YAML language, and then illustrate how it can be used as ELN in NOMAD. 

NOMAD offers a versatile tabular parser that can be configured to process tabular data with different representations, such as data arranged in **columns** or **rows**.

- Columns: each column contains an array of cells that we want to parse into one quantity. Example: current and voltage arrays to be plotted as x and y.
  
- Rows: each row contains a set of cells that we want to parse into a section, i.e., a set of quantities. Example: an inventory tabular data file (for substrates, precursors, or more) where each column represents a property and each row corresponds to one unit stored in the inventory.

More details on the different representations of tabular data can be found [here](https://nomad-lab.eu/prod/v1/staging/docs/howto/customization/tabular.html){:target="_blank"}.

## Steps to Upload and Visualize `.csv` Data


### **Step 1: Defining and Saving the Schema File**

Let's start by creating a new schema file with the `.archive.yaml` format, and create a section called Optical_absorption.

```yaml
definitions:
  name: This is a parser for optical absorption data in the .csv format
  sections:
    Optical_absorption:
```

### **Step 2: Adding the Needed Base Sections**

The next step is to inherit the base sections to meet our ELN needs.

- To create entries from this schema we will use `nomad.datamodel.data.EntryData`
- To use the tabular parser we will use `nomad.parsing.tabular.TableData`
- To enable the plot function we will use `nomad.datamodel.metainfo.plot.PlotSection`

remember, the NOMAD syntax to include sections to inherit from, was `base_sections:`, and also care for the indentation, `base_sections` should be indented with respect to the 'Optical_absorption'.

```yaml
base_sections:
  - nomad.datamodel.data.EntryData
  - nomad.parsing.tabular.TableData
  - nomad.datamodel.metainfo.plot.PlotSection
```

### **Step 3: Defining the Quantities of Our Schema**

We will define the quantities in our ELN schema. Three quantities are needed, let's call them:

- **`data_file`** to upload the data file and apply the tabular parser.
- **`wavelength`** to store x-axis values extracted by the parser.
- **`absorption`** to tore y-axis values extracted by the parser.

and give them a proper type and shape attribute.

```yaml
quantities:
  data_file:
    type: str
  wavelength:
    type: np.float64
    shape: ['*']
  absorbance:
    type: np.float64
    shape: ['*']
```

### **Step 4: Instructing NOMAD on How to Treat Different Quantities**

Remember the syntax for this purpose was `m_annotations:`

* **The `data_file` quantity:**  

The first is to instruct NOMAD to allow for droping and selecting files in this quantity. Here we will use the following: 

```yaml
eln:
  component: FileEditQuantity
```
The second is to instruct NOMAD to open the operating system's data browser to select files:

```yaml
browser:
  adaptor: RawFileAdaptor
```
The third one instructs NOMAD to apply the tabular parser to extract the data from the uploaded file:
```yaml
tabular_parser:
  parsing_options:
    comment: '#'
    skiprows: [1]
  mapping_options:
    - mapping_mode: column
      file_mode: current_entry
      sections:
        - '#root'
```
So we will annotate the `data_file` as following:

```yaml
m_annotations:
  eln:
    component: FileEditQuantity
  browser:
    adaptor: RawFileAdaptor
  tabular_parser:
    parsing_options:
      comment: '#'
      skiprows: [1]
    mapping_options:
      - mapping_mode: column
        file_mode: current_entry
        sections:
          - '#root'  
```


* **The `wavelength` quantity:**  
This quantitiy will accept values, that will be extracted by the tabular parser. Therefore the annotation will be:
```yaml
m_annotations:
  tabular:
    name: Wavelength
```
Note that the value for the `name` key **must** be exactly written as the **header of the column that we want to capture its values** and put in the Wavelength quantity we defined.

* **The `absorbance` quantity:**  
```yaml
m_annotations:
  tabular:
    name: Absorbance
```

### **Step 5: Creating a Plot for Your Data**

To visualize the data from the uploaded and parsed file within the ELN, we will use an annotation for the main section of our schema `Optical_absorption`.
By using the `plotly_graph_object` annotation we instruct NOMAD which quanty to use for the x-axis and which quantities for the y-axis (can be several quantities), as well as provide the title of the plot. Within the plotly_graph_object annotation, the data key defines the quantites for each axis. Here, these varaiable names match those which are defined in the schema. Finally, plot's title is set using the layout key.

```yaml
m_annotations:
  plotly_graph_object:
    data:
      x: "#wavelength"
      y: "#absorbance"
    layout:
      title: Optical Spectrum
```
Note that here, the graph object belongs to the `Optical_absorption` section definition. Therefore, the `m_annotations:` **must be at the same hierarchy level as `quantities`, and `base_sections`**.

### **Step 6 (optional): Adding a Free Text Field**

If you only want to publish your data and graph, consider adding a short description.  
To do this, select a free text field from [editable quantities](https://nomad-lab.eu/prod/v1/gui/dev/editquantity){:target="_blank"} and add it to your schema.  

For example:

```yaml
info_about_data:
  type: str
  m_annotations:
    eln:
      component: RichTextEditQuantity
```

Finally our custom schema file should look like the following. You can also find the **optical_absorptoion_plot_schema.archive.yaml** file in [tutorial_16_materials/part_4_files](https://github.com/siamakn/temp_tutorial_16/tree/main/tutorial_16_materials){:target="_blank"} or download it [here](https://github.com/siamakn/temp_tutorial_16/blob/main/tutorial_16_materials/part_4_files/optical_absorption_plot.archive.yaml){:target="_blank"}.

```yaml
definitions:
  name: This is a parser for optical absorption data in the .csv format
  sections:
    Optical_absorption:
      base_sections:
        - nomad.datamodel.data.EntryData
        - nomad.parsing.tabular.TableData
        - nomad.datamodel.metainfo.plot.PlotSection
      quantities:
        info_about_data:
          type: str
          m_annotations:
            eln:
              component: RichTextEditQuantity          
        data_file:
          type: str
          m_annotations:
            eln:
              component: FileEditQuantity
            browser:
              adaptor: RawFileAdaptor
            tabular_parser:
              parsing_options:
                comment: '#'
                skiprows: [1]
              mapping_options:
                - mapping_mode: column
                  file_mode: current_entry
                  sections:
                    - '#root'
        wavelength:
          type: np.float64
          shape: ['*']
          m_annotations:
            tabular:
              name: Wavelength
        absorbance:
          type: np.float64
          shape: ['*']
          m_annotations:
            tabular:
              name: Absorbance
      m_annotations:
        plotly_graph_object:
          data:
            x: "#wavelength"
            y: "#absorbance"
        layout:
          title: Optical Spectrum
```

### **Step 7: Uploading the Schema File to NOMAD and Creating an Entry**

Now that we have created the ELN schema file for parsing the optical absorption data file, let's put it to the test in the NOMAD GUI.

??? example "Example: Adding Plot to the Polymer Processing custom schema (Steps)"

    Now, let's enhance our Polymer Processing schema.  
    We already had this custom schema file:

    ```yaml
    definitions:
      name: Processing and characterization of polymers thin-films, given by user (gbu)
      sections:
        Experiment_Information_gbu:
          base_sections: 
            - nomad.datamodel.data.EntryData
          quantities:
            Name_gbu:
              type: str  
              default: Experiment title
              m_annotations:
                eln:
                  component: StringEditQuantity 
            Researcher_gbu:
              type: str
              default: Name of the researcher who performed the experiment
              m_annotations:
                eln:
                  component: StringEditQuantity
            Date_gbu:
              type: Datetime
              m_annotations:
                eln:
                  component: DateTimeEditQuantity
            Additional_Notes_gbu:
              type: str
              m_annotations:
                eln:
                  component: RichTextEditQuantity
          sub_sections:
            Sample_gbu:
              section:
                base_sections:
                  - nomad.datamodel.data.EntryData
                  - nomad.datamodel.metainfo.eln.Sample
                m_annotations:
                  eln:
                    overview: true
                    hide: ['chemical_formula']
            Solution_gbu:
              section:
                base_sections:
                  - nomad.datamodel.data.EntryData
                  - nomad.datamodel.metainfo.eln.Sample
                m_annotations:
                  eln:
                    overview: true
                    hide: ['chemical_formula', 'description']
                quantities:
                  Concentration_gbu:
                    type: np.float64
                    unit: mg/ml
                    m_annotations:
                      eln:
                        component: NumberEditQuantity
                sub_sections:
                  Solute:
                    section:
                      base_sections:
                        - nomad.datamodel.data.EntryData
                      quantities:
                        Substance_gbu:
                          type: nomad.datamodel.metainfo.eln.ELNSubstance
                          m_annotations:
                            eln:
                              component: ReferenceEditQuantity
                        Mass_gbu:
                          type: float
                          unit: kilogram
                          m_annotations:
                            eln:
                              component: NumberEditQuantity
                              defaultDisplayUnit: milligram
                  Solvent_gbu:
                    section:
                      base_sections: 
                        - nomad.datamodel.data.EntryData
                      quantities:
                        substance_gbu:
                          type: nomad.datamodel.metainfo.eln.ELNSubstance
                          m_annotations:
                            eln:
                              component: ReferenceEditQuantity
                        Volume_gbu:
                          type: float
                          unit: meter ** 3
                          m_annotations:
                            eln:
                              component: NumberEditQuantity
                              defaultDisplayUnit: milliliter
            Preparation_gbu:
              section:
                base_sections:
                  - nomad.datamodel.data.EntryData
                  - nomad.datamodel.metainfo.eln.Process  
                m_annotations:
                  eln:
                    overview: true
    ```

    
     At the same level of the subsections ´Sample_gbu´, `Solution_gbu`, and  `Preparation_gbu`, we add a new section and name it `Sample_Characterization_gbu`, and tell NOMAD it is an eln using annotations:

       ```yaml
       Sample_Characterization_gbu:
         section:
           m_annotations:
             eln:
       ```

    **(Almost) similar to Step 1: We start with the Optical_absorption section.**  
       Here, to generalize our schema, we assume we want to characterize our samples with several techniques, e.g., electrical conductivity and optical absorption. Therefore, we introduce subsections to our `Sample_Characterization_gbu` (i.e. a placeholder for other characterization data) and introduce our Optical_absorption_gbu as a subsection:
       ```yaml
       sub_sections:
         Optical_absorption_gbu:
           section:
       ```  

    **Similar to Step 2, we add base sections**  
       - Remember we always need to add the basesections `nomad.parsing.tabular.TableData` and `nomad.datamodel.metainfo.plot.PlotSection` to be able to parse the data, and plot them, respectively. However, this time, we benefit from a *more specialized* NOMAD base section, `nomad.datamodel.metainfo.basesections.Measurement`, that gives us more functionalities than just making an entry. Using the `nomad.datamodel.data.EntryData` is not needed anymore, because it is already inherited in `nomad.datamodel.metainfo.basesections.Measurement`. So we add:
       ```yaml
       base_sections:
         - nomad.datamodel.metainfo.basesections.Measurement
         - nomad.parsing.tabular.TableData
         - nomad.datamodel.metainfo.plot.PlotSection
       ```  
       - Tip (optional): we can choose a lable for eln sections using the following syntax:
       ```yaml
       lable: Optical Absorption
       ```   
    **Similar to Step 3, we define our 3 desired quantities, and give their type and shape values**
    ```yaml
    quantities:
      data_file_gbu:
        type: str
      wavelength:
        type: np.float64
        shape: ['*']
      absorbance:
        type: np.float64
        shape: ['*']
    ```
    **Similar to Step 4, we instruct NOMAD how to treat each of these quantities, using annotations**   

    For `data_file_gbu`:
    ```yaml
    m_annotations:
      eln:
        component: FileEditQuantity
      browser:
        adaptor: RawFileAdaptor
      tabular_parser:
        parsing_options:
          comment: '#'
          skiprows: [1]
        mapping_options:
          - mapping_mode: column
            file_mode: current_entry
            sections:
              - '#root'  
    ```
    For wavelength and absorbance quantities:
    ```yaml
    m_annotations:
      tabular:
        name: Wavelength
    ```
    and
    ```yaml
    m_annotations:
      tabular:
        name: Absorbance
    ```
    **Similar to Step 5, we creating a plot for our data.**

    We again note that here, the graph object belongs to the `Optical_absorption_gbu` section definition. Therefore, the `m_annotations:` **must be at the same hierarchy level as `quantities`, and `base_sections`** of this section.
    ```yaml
    m_annotations:
      plotly_graph_object:
        data:
          x: "#wavelength"
          y: "#absorbance"
        layout:
          title: Optical Spectrum
    ```
    Finally our custom schema file should look like the following. You can also find the **polymwe_processinf_and_optical_absorptoion_plot_schema.archive.yaml** file in [tutorial_16_materials/part_4_files](https://github.com/siamakn/temp_tutorial_16/tree/main/tutorial_16_materials){:target="_blank"} or download it [here](https://github.com/siamakn/temp_tutorial_16/blob/main/tutorial_16_materials/part_4_files/polymer_processing_and_optical_absorption_plot.archive.yaml){:target="_blank"}.