# pyBigWig-0.3.19
 A new version of **[`pyBigWig`](https://github.com/deeptools/pyBigWig)**. Compared with the previous version, the new version improves the way handling null values in bigwig files.
## Installation 
You can install this extension directly from github with:

     pip install git+https://github.com/1944498970/pyBigWig.git

## Usage 
 The usage is same with the previous version, you can read the [readme.md](https://github.com/deeptools/pyBigWig/blob/master/README.md) of previous version.
## Differences from previous versions
If our bedgraph file is like:
```
track type=bedGraph name="BedGraph Format" description="BedGraph format" visibility=full color=200,100,0 altColor=0,100,200 priority=20
chr1 4 8 -1.0
chr1 16 20 -0.75
chr1 25 30 -0.50
```
We turn this file into bigwig format named `test.bigwig`.Then we use pyBigWig to open it:

    import pyBigWig
    bw=pyBigWig.open('test.bigwig')
Using some commands of pyBigWig to handle it, the final results of two versions will be list as follows.
| command |pyBigWig-0.3.19 |pyBigWig-0.3.18 |
| :---------:| :----: | :----: |
| bw.values('chr1' , 1, 4) | [0.0, 0.0, 0.0] | [nan, nan, nan] |
| bw.stats('chr1' , 1, 4)| [0.0] | [None] |
|bw.stats('chr1' , 4, 8)|[-1.0]|[-1.0]|
| bw.stats('chr1' , 4, 16)| [-0.3333333333333333] | [-1.0] |
| bw.stats('chr1' , 1, 4,nBins=2) | [0.0,  0.0] | [None, None]|
|bw.stats('chr1' , 4, 8,nBins=2)|[-1.0, -1.0]|[-1.0, -1.0]|
| bw.stats('chr1' , 4, 16,nBins=4)) |[-1.0, -0.3333333333333333, 0.0, 0.0] | [-1.0, -1.0, None, None] |
According to the list we can find the previous version can't handle the site without value, so when we get its value it will print `nan`,and when we calculate the mean of the region of `[4,16]` the previous version will ignore sites without values so it will print `-1.0` which is not same as the true mean. 