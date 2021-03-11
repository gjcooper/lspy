# Proces for translating

Add to toc

## Headings

Make sure all lines starting with #Heading and translated to # Heading

Labels `{#labelname}` become `(labelname)=` and move from the enf of the heading to the previous line.

## Footnotes
`^[txt]` becomes `[^key]` and then at end of page `[^key]: txt`

vim search: ?\v\^[.{-}\] (first three chars from typing /)
Macro to replace: ll"bdi[a^[<80><fd>aikey^[<80><fd>aNxphyf]Go^[<80><fd>aPA: ^[<80><fd>a"bp^On
Move to footnotes at bottom and give them appropriate labels
Move to first footnote at bottom - make sure [^key] is search term
Macro to correct keys: wyt]nwPldt]^Oj0

## Citations
`[@citekey]` becomes `citetxt{cite}```citekey``

Make sure to put references at end with some similar to: 

````
## References

```{bibliography} ../refs.bib
```
````
```
## Automatically generated figures

Can use glue: https://jupyterbook.org/content/glue.html - especially if you need to reference it later

## References within document

`\@ref(tab:simpsontable)` becomes `{ref}```simpsontable`` or `{numref}```simpsontable``

## Code example conversion

Replace R code cell designation to MYST code-cell, with ipython3 backend

vim search and replace 
 - :%s/{r}/\{code-cell\} ipython3/
 - :%s/{r error=TRUE}/{code-cell} ipython3\r:tags: ["raises-exception"]/
Build book and fix all code errors
* All `<-` should be replaced by `=`
* All c(x,y,z) should be replaced by [], or np.array([])
* All dots in name should be replaced by underscore
* All TRUE/FALSE replaced by True/False

## Fix any GCNOTE sections
