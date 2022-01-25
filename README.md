An example of a CSV on the Web produced using data taken from the Department for Education's _Explore Education Statistics (EES)_ service. Specifically, the file _LA - Free school meals (FSM)_ dataset found [here](https://explore-education-statistics.service.gov.uk/data-catalogue/outcomes-for-children-in-need-including-children-looked-after-by-local-authorities-in-england/2020).

- [`fsm_la.csv`](fsm_la.csv) and [`data-guidance.txt`](data-guidance.txt) are files containing the statistical data and metadata as proivded by the EES service.
- [`metadata.json`](metadata.json) contains extended metadata for the dataset as JSON-LD. It makes use of the [DCAT](https://w3c.github.io/dxwg/dcat/) attempts to structure the EES catalogs and datasets according to the spec. It also includes CSV on the Web metadata embedded within the JSON-LD to describe CSV as a distribution of a dataset.
- [`fsm_la.csv-metadata.json`](fsm_la.csv-metadata.json) is valid CSV on the Web (CSVW) metadata which controls the transformation of the CSV in the RDF Linked Data.
- [`output.ttl`](output.ttl) is the output following transformation from CSV into RDF using Swirrl's [`csv2rdf`](https://github.com/Swirrl/csv2rdf) tool.

Some thoughts/issues:

- Lots of potential!
- Many of the columns included in the CSV file are suppressed in the RDF ouput as they provide names/labels for the columns using statistical codes.
- The `t_pupils`, `t_FSM_eligible` and `pt_FSM_eligible` columns mix numeric and string values into a single column, the `c`'s being used to indicate suppressed results. Mixing types in a single column causes some difficulty as numeric results will be coerced into being represented as strings during transformation. 
- In this example, I have explicitly indicated in the metadata that these columns _should_ have a numeric datatype, but this would cause the CSV to fail validation, and also causes records where `c`'s are used to not appear in the RDF output.
- The CSV doesn't use the measure-dimension approach (i.e., pivoted) to represent the data. Using measure-dimensions is our current way of overcoming the problem of mixing types in a single column.

| ... | measure_type    | value | marker | units                |
|-----|-----------------|-------|--------|----------------------|
| ... | t_pupils        | 100   |        | number-of-pupils     |
| ... | t_FSM_eligible  |       | c      | number-of-pupils     |
| ... | pt_FSM_eligible |       | c      | percentage-of-pupils |

- The [`data-guidance.txt`](data-guidance.txt) contains a lot of metadata which we could find an appropriate home for. The explanations of codes used within the dataset could be handled by creating an RDF codelist.