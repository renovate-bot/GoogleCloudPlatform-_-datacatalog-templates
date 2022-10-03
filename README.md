## datacatalog-templates

This repository contains sample tag templates in yaml-format. It also contains two utilities: 
1. [`create_template.py`](create_template.py), which creates a tag template in Data Catalog from a yaml file. 
2. [`evolve_template.py`](evolve_template.py), which evolves an existing tag template in Data Catalog from a yaml file.  


### Dependencies

Before running `create_template.py` or `evolve_template.py`, install the `pyyaml` and `google-cloud-datacatalog` modules as follows:

```
pip install pyyaml
pip install google-cloud-datacatalog
```

### Running the `create_template.py` script

```
export GOOGLE_APPLICATION_CREDENTIALS='my_keyfile.json'
export PROJECT='my-project-id'
export REGION='us-central1'
export YAML='data_quality_v1.yaml'

python create_template.py $PROJECT $REGION $YAML
```

### Running the `evolve_template.py` script

The [`evolve_template.py`](evolve_template.py) script can be run in either "validate" or "apply" mode. <br>
In "validate" mode, the script performs a diff between the existing tag template in Data Catalog and the tag template representation in the yaml file. It reports all the changes it would make to the tag template without actually making them. <br>
In "apply" mode, the changes are applied to the tag template in Data Catalog such that it is brought up-to-date with its yaml representation.<br> 

```
export GOOGLE_APPLICATION_CREDENTIALS='my_keyfile.json'
export MODE='validate'
export PROJECT='my-project-id'
export REGION='us-central1'
export YAML='data_quality_v2.yaml'

python evolve_template.py $MODE $PROJECT $REGION $YAML
```

### Supported tag template changes

- Adding a new field 
- Deleting a field
- Renaming a field 
- Reordering a field
- Adding an enum value
- Deleting an enum value
- Renaming an enum value
- Adding, updating and deleting a field description
- Updating a field's display name
- Changing a required field to optional
- Renaming a field

To rename a field, use the syntax:<br>
current_name:new_name<br>

For example, to rename the field `constraint_type` to `constraint_name`:<br>
```
    - field: constraint_type:constraint_name
```

Similarly, to rename an enum value, use the syntax:<br>
current_value:new_value<br>

For example, to rename the enum value `Not Null` to `NOT NULL`:<br>
```
    - field: constraint_type
      type: enum
      values: PK|FK|Not Null:NOT NULL|Unique
```

### Unsupported tag template changes

- Changing an optional field to required
- Changing the datatype of a field

### Tag template examples

The repository comes with a number of sample tag templates for different subject areas ranging from data discovery to data governance. These are meant as inspiration and should be customized to your organization's vocabulary and needs. 

To test the [`evolve_template.py`](evolve_template.py) script, we have prepared a couple different variations of the data quality template so that you can see the effects of the script as the tag template evolves. Run the sample templates as follows:

```
python create_template.py $PROJECT $REGION data_quality_v1.yaml
python evolve_template.py validate $PROJECT $REGION data_quality_v2.yaml 
python evolve_template.py apply $PROJECT $REGION data_quality_v2.yaml 
python evolve_template.py validate $PROJECT $REGION data_quality_v3.yaml 
python evolve_template.py apply $PROJECT $REGION data_quality_v3.yaml
```

### Contributing

See the contributing [instructions](/CONTRIBUTING.md) to get started
contributing.


### License

All solutions within this repository are provided under the
[Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license. Please see
the [LICENSE](/LICENSE) file for more detailed terms and conditions.


### Disclaimer

This repository and its contents are not an official Google Product.
