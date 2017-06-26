# Simple Genome App Guide

## Basic Steps

1. Modify `template.input.ga`
2. Run the Simple Genome App using `run_genome_app.py`
3. Make GitHub repository
4. Create a `README` for the Simple Genome App


## Walkthrough

Let's make a Simple Genome App to check your muscle type!<br>

## 1. Modify Simple Genome App Input file

1. Open and modify the `template.input.ga` file with [Microsoft Excel](https://products.office.com/en-us/excel) or any other spreadsheet editor.
    * Learn a bit about Guardiome's `ga` file format [here](#ga-file-format).
2. Delete the additional columns (`Cause`, `Facts`) and the lines underneath the header in the `template.input.ga` file.
    * The rows in the `template.input.ga` file contain all of the input cases that are valid in the Simple Genome App.
3. Find information about genes and variants associated with muscle type.
    * Here is an entry in [SNPedia](https://www.snpedia.com/) about variant [rs4988235](https://www.snpedia.com/index.php/Rs1815739)
    associated with muscle type.
4. Add this information to the specified columns in the `template.input.ga` file:
    * FEATURE: `rs1815739`
    * FEATURE TYPE: `variant`
    * STATE: `T|T`
    * REGION: `11:66560624-66560624`
    * RESULT: `A person with these genomic features may have muscles with impaired performance and likely prefer
    endurance over sprinting. Their fast-twitch muscle fibers likely do not have much alpha-actinin 3 protein.`
    * REFERENCE: `Roth SM, Walsh S, Liu D, Metter EJ, Ferrucci L, Hurley BF. The ACTN3 R577X nonsense allele is under-represented in elite-level strength athletes. Eur J Hum Genet. 2008;16(3):391-4.`
    * NOTE: All of the genotypes MUST be with respect to the positive strand because alleles in the VCF file are with respect
    to the positive strand. [Learn More]()
5. (Optional) Add any information in additional columns specific to each result.
    * For example: Make a `Cause` column and add the following text to that row: `A variant causes
    a premature stop codon in both copies of the ACTN3 gene, responsible for producing alpha-actinin-3 protein.`
    * Do not add additional columns with names of any mandatory columns in our GA file format.
6. (Optional) Repeat steps 4-5 for different results.
    * In the next row, add the following information:
        * FEATURE: `rs1815739`
        * FEATURE TYPE: `variant`
        * STATE: `C|T`
        * REGION: `11:66560624-66560624`
        * RESULT: `A person with these genomic features may have muscles with average performance and likely prefer
        sprinting over endurance. Their fast-twitch muscle fibers likely have some alpha-actinin 3 protein.`
        * Cause: `A variant causes a premature stop codon in one copy of the ACTN3 gene, responsible for producing alpha-actinin-3 protein.`
7. Add the default result in the following format to the header (above the column names)
    * `# RESULT.default=A person with these genomic features may have muscles that perform better than the average
    person and they likely have improved sprinting ability. Their fast-twitch muscle fibers likely have the
    alpha-actinin 3 protein.`
    * The default result will be returned if none of the features listed in the file are found.
8. (Optional) Add the default information for any additional columns to the header
    * `# Cause.default=This variant was not found in the ACTN3 gene, responsible for producing alpha-actinin-3 protein.`
9. (Optional) Add descriptions for each result type (result and additional columns) to the header
    * `# RESULT.description=The result that was found based on the features searched in the VCF file.`
    * Result descriptions appear in help dialogue boxes when a user clicks for more information about a specific result type in Genome AI.

You have just created the Simple Genome App Input file for muscle type. Now you are ready to run the muscle type Simple Genome App!

## 2. Run the Simple Genome App

To run the muscle type Simple Genome App, run the following command in the `tools` folder:

```sh
python run_genome_app.py ../input/genome-app-template.input.ga
```
It will create a `genome-app-name.output.ga` file inside the output folder. Learn more about the `.output.ga` file
[here](#simple-genome-app-output).


## 3. Make GitHub repository

All Genome Apps live on GitHub, like [this one](https://github.com/Guardiome/muscle-type.git), so the Guardiome Genome App store can access them.

1. Make a Github Account [here](https://github.com/join?source=login) if you don't already have one.
2. Make a new repository by clicking the green "New" button on your GitHub profile.
   * Name the repository the name of your Genome App
   * Make the repository public
   * Dont initialize the repository with a README
3. Download your repository
```sh
git clone https://github.com/user-name/repository-name.git
```
4. Move __everything__ in your Genome App folder into the repository folder
5. Add your repository changes to GitHub
```sh
cd 'repository'
git add -A
git commit -m "added Genome App"
git push
```
6. Go look at your Genome App on Github!



## 4. Create a README for the Simple Genome App

Create a `README` file for the Simple Genome App by either modifying this
[file](https://github.com/Guardiome/create-genome-app/blob/master/simple-genome-app/README.md) or making your own in
the `genome-app-name` folder. Here is an [example](https://github.com/Guardiome/muscle-type/blob/master/README.md)
of a `README` for the muscle type Simple Genome App.

The `README` should contain basic information about the Simple Genome App in general regardless of the result.
Here is a list of information about the Simple Genome App that you may want to add:

* Description
* Biology
* Genetics
* Risk Factors

The `README` created for each Genome App will be shown with the result in the Genome AI.

# GA File Format

GA (genome-app) is Guardiome's file format for representing genotype-to-phenotype relationships. Our GA file
contains the following information:

* [Feature](#feature)
* [Feature-type](#feature-type)
* [State](#state)
* [Region](#region)
* [Result](#result)
* [Reference](#reference)
* [Default](#default)
* [Description](#description)
* [Additional Columns](#additional-columns)

For more information, refer to our GA file format [specification](https://github.com/Guardiome/create-genome-app/blob/master/GA_v1.0_specification.pdf).

### FEATURE

The `FEATURE` column contains variants, genes, or both. The following are all valid entires in this column:

* `rs1815739`
* `ACTN3`
* `rs1815739;ACTN3`

### FEATURE-TYPE

The `FEATURE TYPE` column contains the type of each feature. The following are all valid entries in this column:

* `variant`
* `gene`
* `variant;gene`

## STATE

The `STATE` column contains the state of each feature associated with a specific result. The following are all valid
entries in this column:

* `T|T`, `C`
* `HIGH`
* `T|T;HIGH`

The state of a variant can either be the genotype of one allele (i.e. `A`) or the genotype of both alleles (i.e. `G|G`).
The genotype must __ALWAYS__ be with respect to the __positive strand__ because VCF files specify the REF and ALT
alleles based on the positive strand.

The state of a gene can either be a stringency filter option from most stringent to least stringent:
('HIGH', 'MODERATE', 'LOW') - default stringency is set to 'HIGH' or a specific state based on the effect of the variants
in the gene. A list of valid gene states can be found [here](http://snpeff.sourceforge.net/SnpEff_manual.html#input) under
the `Effect Prediction Details` header (scroll down few pages).

For a list of gene states under in each stringency level, refer to the table in our GA file format
[specification](https://github.com/Guardiome/create-genome-app/blob/master/GA_v1.0_specification.pdf).

## REGION

The `REGION` column contains the chromosomal location of each feature. The following are all valid entries in this column:

* `11:66560624-66560624`
* `11:66546395-66563329`
* `11:66560624-66560624;11:66546395-66563329`

## RESULT

The `RESULT` column contains the phenotype associated with the state of each feature. For example: If you have the variant `rs1815739` with genotype `T|T` at the region `11:66560624-66560624`, your phenotype will be `Your muscles have impaired performance and you likely prefer endurance over sprinting. Your fast-twitch muscle fibers likely do not have much alpha-actinin 3 protein.`

Each row in `template.input.ga` represents a specific result.

## REFERENCE

The `REFERENCE` column contains the list of references supporting the genotype to phenotype association. References
can be in any format: MLA, APA, URL. The only requirement is that the references are separated by a vertical line ("|").

*`Roth SM, Walsh S, Liu D, Metter EJ, Ferrucci L, Hurley BF. The ACTN3 R577X nonsense allele is under-represented in elite-level strength athletes. Eur J Hum Genet. 2008;16(3):391-4.`|`https://www.snpedia.com/index.php/Rs1815739`

## Default

The `XXX.default` rows contains the default values associated with having the reference genotype in all of the features checked. If none of the features searched in their respective states are found in the VCF file, all of the default values in the header will be returned.

For example: If you do not have the variant `rs1815739` you will have the reference genotype at this position,
which is `C|C`. The `C|C` genotype is associated with high muscle performance. Therefore, having the reference genotype means your muscles will have high performance.

You can also add additional default information specific to the `Result.default`. The only default information you can display are for: (`Result` and any additional columns specified in the file). All default information must be in the following format:

* `# XXX.default=Default value of XXX.`
* `XXX` = Column Name

Make sure that the column name in the `.input.ga` file is exactly the same as in the default row in the header.

## Description

The `XXX.description` rows contain descriptions for the data types (columns) in the `.ga` files. These descriptions will appear in help dialogue boxes in the Genome AI whenever a user wants to learn more about each result type.

The only descriptions you can display are for: `Result` and any additional columns specified in the file. All descriptions must be in the following format:

* `# XXX.description=Description of XXX.`
* `XXX` = Column Name

Make sure that the column name in the `.input.ga` file is exactly the same as in the description row in the header.

## Additional Columns

You can add additional information to the `template.input.ga` by adding additional columns after the `RESULT` column.
Now you can add values in each additional column specific to each result. Here are a few examples of additional columns
you may want to add:

* Action
* Cause
* Facts

Do not add any additional columns of any column already being used in the GA file format:

* Feature
* Feature type
* Result
* Region
* State
* Reference

# Simple Genome App Output

The Simple Genome App output file `genome-app-name.output.ga` is in the same file format as the Simple Genome App input
file `template.input.ga` except for two changes:

* The default headers are removed from the file.
* All of the rows are removed from the GA file if the feature specified is not found in the VCF file.
* If all features are removed, the information from the default headers are added as a row in the file.

The order which the Simple Genome App Output results are shown in the Genome AI is dependent on the order of the columns:

* Result
* Additional Column 1
* Additional Column 2

<div>
    <img src="./simple-genome-app/media/guardiome-logo.png" align="center" width=400 height=150>
</div>
