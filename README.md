# siesta-action 
I use the [latest stable release of siesta](https://gitlab.com/siesta-project/siesta/-/tree/rel-4.1) [Siesta](https://gitlab.com/siesta-project/siesta) and using github action perform small DFT calculations.
### Usage:
Keep your directory structure similar to the following. 

**Note** : The only neccessary files are the:
  - input-directory with the input files
  - .github/workflows/ directory with the Your-worflow-file.yml

```
GitHub Repository
├── input-directory
│   ├── 
│   ├── input-file.fdf
│   └── psf files
└── .github
    └── workflows
        └── Your-worflow-file.yml
```

The various input parameters are as follows:

```yaml
file-name:
  description: 'Name of the input file(with extention)'
  default: 'ip.fdf'
  required: true

dir-name:
  description: 'Name of the directory(folder) input files'
  default: 'ip'
  required: true

out-name:
  description: 'Name of the output file'
  default: 'ip.out'
  required: true
 ```
 #### A few important points before we use the action
 - action/checkout@v2 should be used before this action to clone the input files
 - actions/upload-artifact@v2 should be used after this action to save all the output

Here's a sample workflow which can be used directly:
```yaml
name: Manually triggered workflow To Run Siesta
on:
  workflow_dispatch:
    inputs:
      file-name:
        description: 'Name of the input file(with extention)'
        default: 'ip.fdf'
        required: true

      dir-name:
        description: 'Name of the directory(folder) input files'
        default: 'ip'
        required: true

      out-name:
        description: 'Name of the output file'
        default: 'ip.out'
        required: true
jobs:
  DFT-calculations-Siesta:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v2
      - name: siesta-action
        uses: ShantanuDash/siesta-action@v1.0
        with:
            file-name: ${{ github.event.inputs.file-name }}
            dir-name: ${{ github.event.inputs.dir-name }}
            out-name: ${{ github.event.inputs.out-name }}
       
      - name: Upload Results
        uses: actions/upload-artifact@v2
        with:
            name: DFT-Output
            path: ${{ github.event.inputs.dir-name }}/
            
```

After you have added the input files and the workflow file, you can trigger/run the workflow as shown below:
![Step 1](assets/Image 1.png)
