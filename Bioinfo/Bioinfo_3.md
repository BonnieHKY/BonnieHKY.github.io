# Mapping

---

## STAR

1. 下载[STAR]([GitHub - alexdobin/STAR: RNA-seq aligner](https://github.com/alexdobin/STAR))，按照README进行安装
   
   ```
   # Get the latest STAR source from releases
   wget https://github.com/alexdobin/STAR/archive/2.7.10b.tar.gz
   tar -xzf 2.7.10b.tar.gz
   cd STAR-2.7.1
   
   # Alternatively, download from https://github.com/alexdobin/STAR/archive/2.7.10b.tar.gz
   # And share the pakage to Linux
   ```

2. 在Linux下安装
   
   ```
   cd STAR-2.7.10b/source
   make STAR
   # A few minutes are needed to process
   ```

3. 使用一个拟南芥RNA-seq数据在叶绿体的序列违示例
   
   - 下载所需的数据[STAR.mapping.tar.gz]([清华大学云盘 (tsinghua.edu.cn)](https://cloud.tsinghua.edu.cn/d/429647f35add41338d1a/))
   
   - 建立STAR index：
   
   ```
   tar xvzf STAR.mapping.tar.gz
   mkdir tair10.Pt.STARindex
   STAR-2.7.10b/bin/Linux_x86_64_static/STAR --runMode genomeGenerate --genomeFastaFiles data/tair10.Pt.fa --sjdbGTFfile data/tair10.Pt.gtf --genomeDir tair10.Pt.STARindex --genomeSAindexNbases 7
   ```
   
   - mapping
     
     在输出目录中，ath.alignedAligned.out.sam是mapping产生的sam文件
     
      ath.alignedSJ.out.tab是splice junction的信息，其余是STAR输出的日志文件
   
   ```
   mkdir output
   STAR-2.7.10b/bin/Linux_x86_64_static/STAR --runMode alignReads --genomeDir tair10.Pt.STARindex --readFilesIn data/ath_1.fastq data/ath_2.fastq --outFileNamePrefix output/ath.aligned
   
   ll output/
   total 13296
   drwxr-xr-x 2 test test     4096 Apr  5 13:02 ./
   drwxr-xr-x 1 test test     4096 Apr  5 13:00 ../
   -rw-r--r-- 1 test test 13582309 Apr  5 13:02 ath.alignedAligned.out.sam
   -rw-r--r-- 1 test test     1987 Apr  5 13:02 ath.alignedLog.final.out
   -rw-r--r-- 1 test test     4311 Apr  5 13:02 ath.alignedLog.out
   -rw-r--r-- 1 test test      246 Apr  5 13:02 ath.alignedLog.progress.out
   -rw-r--r-- 1 test test     1078 Apr  5 13:02 ath.alignedSJ.out.tab
   ```
   
   - 如果不想每次都输入前面的一串路径，可以在`~/.bashrc`中将bedtools所在的目录加入环境变量：
   
   ```
   echo "export PATH=$PATH:/home/test/mapping/STAR-2.7.10b/bin/Linux_x86_64_static" >> ~/.bashrc
   source ~/.bashrc
   which STAR
   # print out "/home/test/mapping/STAR-2.7.10b/bin/Linux_x86_64_static/STAR
   ```

## Genome Browser

- Genome Browser是用于显示来自生物学数据库的基因组数据信息的图形界面，用于可视化展示基因的突变、可变剪接、上下游基因、区域保守性、重复元件

-  Integrative Genomics Viewer (IGV) 是一个强大的基因组可视化工具
