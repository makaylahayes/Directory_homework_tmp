Directory\_homework
================
Makayla Hayes
2/4/2021

``` r
#First load in HTML verson on website
simple<-read_html("https://guide.wisc.edu/faculty/")
sample
```

    ## function (x, size, replace = FALSE, prob = NULL) 
    ## {
    ##     if (length(x) == 1L && is.numeric(x) && is.finite(x) && x >= 
    ##         1) {
    ##         if (missing(size)) 
    ##             size <- x
    ##         sample.int(x, size, replace, prob)
    ##     }
    ##     else {
    ##         if (missing(size)) 
    ##             size <- length(x)
    ##         x[sample.int(length(x), size, replace, prob)]
    ##     }
    ## }
    ## <bytecode: 0x7feda02973d8>
    ## <environment: namespace:base>

``` r
#Next I changes all the <br> to <p>\n in order to be able to space
#the different categories for the individual person
xml_find_all(simple,".//br")%>%
  xml_add_sibling("p","\n")
xml_find_all(simple,".//br")%>%
  xml_remove()  

#Then I pull out each line of text which is equivent to each persons information
text_out<-simple%>%
  html_nodes(".uw-people")%>%
  html_nodes("li")%>%
  html_nodes("p")
head(text_out)
```

    ## {xml_nodeset (6)}
    ## [1] <p>AARLI,LISA<p>\n</p>Lecturer<p>\n</p>Counseling Psychology<p>\n</p>EDM  ...
    ## [2] <p>\n</p>
    ## [3] <p>\n</p>
    ## [4] <p>\n</p>
    ## [5] <p>\n</p>
    ## [6] <p>ABBOTT,DANIEL E<p>\n</p>Assoc Professor (CHS)<p>\n</p>Surgery<p>\n</p> ...

``` r
#Then I take out all the extra lines
bad<-list()
good<-list()
k=1
j=1
for (i in 1:length(text_out)) {
  if(html_text(text_out[i])=="\n"){
        bad[k]<-html_text(text_out[i])
        k=k+1
  }
else{
  good[j]<-html_text(text_out[i])
  j=j+1
}
}

#Then I extract each person name,job,field,PHD information and
#Put them into separate lists
good<-unlist(good)
names<-list()
job<-list()
field<-list()
PHD<-list()
for(i in 1:length(good)){
 temp<-unlist(strsplit(good[i],"\n"))
 if(length(temp)==4){
   names[i]<-temp[1]
   job[i]<-temp[2]
   field[i]<-temp[3]
   PHD[i]<-temp[4]
 }
 else if (length(temp)==3){
   names[i]<-temp[1]
   job[i]<-temp[2]
   field[i]<-temp[3]
   PHD[i]<-""
 }
}
#Lastly I convert the lists to vectors and
#store them all into a dataframe for convenience
Name<-c(1:3988)
Position<-c(1:3988)
Department<-c(1:3988)
Degree<-c(1:3988)
for(i in 1:3988){
  Name[i]<-unlist(names[i])
  Position[i]<-unlist(job[i])
  Department[i]<-unlist(field[i])
  Degree[i]<-unlist(PHD[i])
}
directory<-data.frame(Name,Position,Department,Degree)
head(directory)
```

    ##                 Name              Position                 Department
    ## 1         AARLI,LISA              Lecturer      Counseling Psychology
    ## 2    ABBOTT,DANIEL E Assoc Professor (CHS)                    Surgery
    ## 3    ABBOTT,DAVID H.             Professor    Obstetrics & Gynecology
    ## 4    ABBOTT,NICHOLAS             Professor Chemical & Biological Engr
    ## 5 ABD-ELSAYED,ALAA A  Asst Professor (CHS)             Anesthesiology
    ## 6   ABDUALLAH,FAISAL   Associate Professor                        Art
    ##                                Degree
    ## 1  EDM 2000 Univ of Wisconsin-Madison
    ## 2    MD 2016 University of Washington
    ## 3    PHD 1979 University of Edinburgh
    ## 4 PHD 1991 Massachusetts Inst Of Tech
    ## 5                            MD 2000 
    ## 6       PHD 2012 Royal College of Art

<https://github.com/makaylahayes/Directory_homework_tmp>
