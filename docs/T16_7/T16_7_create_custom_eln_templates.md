## Guidelines for Building a Custom ELN Schema

To build a custom NOMAD ELN, you need to use the NOMAD metainfo schema language, primarily written in YAML. You custom schema file must have the ending extension `archive.yaml`

??? info "1. NOMAD's archive.yaml files start with the `definitions` keyword, and must have a `name`, and can have a `description`."
    
    NOMAD syntax is:
    ```yaml
    definitions:
      name:
      description:
    ```

    Example:
    ```yaml
    definitions:
      name: My NOMAD ELN
      description: This is an electronic lab notebook schema that includes several sections.
    ```
??? info "2. A schema can have several sections."

    NOMAD syntax is:

    ```yaml
    definitions:
      name:
      description: 

      sections:
        My_first_section:
        My_second_section:
        My_third_section:
    ```


??? info "3. Sections can inherit structure and definition from NOMAD's `base_sections` or other sections."
    When inheriting from a NOMAD base section, use the `base_sections` keyword and list the desired base sections you would like to inherit from. The keyword `base_sections` additionally allows you to also inherit from other sections (e.g., within the same schema or even a section that have been published in NOMAD, see [schema package references in NOMAD Documentation](https://nomad-lab.eu/prod/v1/docs/howto/customization/basics.html#schema-package-references){:target="_blank"}. Inherited sections can be given in a python list, or subsequent indented lines starting with a dash, `-`.

    NOMAD syntax is:
    ```yaml
    definitions:
      name: My NOMAD ELN
      description: Custom ELN schema.

      sections:
        My_first_section:
          base_sections:
            - nomad.datamodel.data.EntryData
            - nomad.datamodel.metainfo.eln.Sample
    ```

    or alternatively in the form of a Python list:
    ```yaml
    definitions:
      name: My NOMAD ELN
      description: Custom ELN schema.

      sections:
        My_first_section:
          base_sections: ['nomad.datamodel.data.EntryData', 'nomad.datamodel.metainfo.eln.Sample']
    ``` 

??? info "4. Sections can contain quantities, other sections, and subsections"
    Each section contains a set of quantites that need to be captured by the ELN. The quantities represent the parameters of your measurement or processing conditions. In addition, sections can also **include** subsections. 

    NOMAD syntax is:

    ```yaml
    definitions:
      name: My NOMAD ELN
      description: This is an electronic lab notebook schema that includes several sections.

      sections:
        My_first_section:
          base_sections:
            - nomad.datamodel.data.EntryData
            - nomad.datamodel.metainfo.eln.Sample
          quantities:

          sub_sections:
    ```


??? info "5. Quantities are defined with type, shape and unit properties"
    Quantities define possible primitive values. The first line in the quantity definition includes a user-defined name for the quantitiy. The basic properties that go into a quantity definition are `type`, `shape`, and `unit`.

    ```yaml
    definitions:
      name: My NOMAD ELN
      description: This is an electronic lab notebook schema that includes several sections.

      sections:
        My_first_section:
          base_sections:
            - nomad.datamodel.data.EntryData
            - nomad.datamodel.metainfo.eln.Sample
          quantities:
            first_quantity:
              - type: #For example, str or np.float64
              - shape: #For example scalar or list (['*'])
              - unit: #For example, meters, amperes, or seconds
          sub_sections:
    ```

??? info "6. Section and quantities can have annotations"
Annotations provide additional information that NOMAD can use to alter its behavior around these definitions and how users can interact with them. The keyword for annotations is `m_annotations`.
```yaml
definitions:
  name: My NOMAD ELN
  description: This is an electronic lab notebook schema that includes several sections.

  sections:
    My_first_section:
      base_sections:
        - nomad.datamodel.data.EntryData
        - nomad.datamodel.metainfo.eln.Sample
      quantities:
        first_quantity:
          - type: #For example, str or np.float64
          - shape: #For example scalar or list (['*'])
          - unit: #For example, meters, amperes, or seconds
          m_annotations:
            annotation_name:
              key1: value1  
      sub_sections:
```

