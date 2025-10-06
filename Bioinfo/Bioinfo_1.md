# 文件格式

---

## GTF基因注释文件

- GTF全称为gene transfer format，主要是用于对基因进行注释。

- GTF文件由9列数据组成，以Tab键作为分割。
  
  1. seq_id：序列的编号，一般为chr或者scanfold编号
  
  2. source：注释的来源，一般为数据库或者注释机构，如果未知则用`.`代替
  
  3. type (feature)：注释信息的类型，比如gene、cDNA、mRNA、CDS、start_codon、stop_codon等
  
  4. start：该基因或者转录本在参考序列上的起始位置
  
  5. end：该基因或者转录本在参考序列上的终止位置
  
  6. score：得分，是注释信息可能性的说明；可以是序列相似性比对时的E-values，或者基因预测时的P-values，
  
  7. strand：表示该基因或者转录本位于参考序列的正义链(+)或者反义链(-)上。
  
  8. phase：仅对注释类型为CDS有效，表示起始编码的位置，有效值为0、1、2，表示对于这一CDS序列来说，下一个密码子开始的碱基位置（不一定是蛋白的翻译起始位置，因为这段序列可能只对于完整肽链的某一部分）
  
  9. attributes：一个包含众多属性的列表，格式为“标签 = 值”，标签与值之间通过空格分开，每个特征之后都有分号（包括最后一个）。例如：`gene_id "YDL248W"; gene_version "1"; gene_name "COS7"; gene_source "ensembl"; gene_biotype "protein_coding";`

## Fastq, Fasta测序文件

### Fastq (.fq)

- Fastq格式是一种基于文本的存储生物序列和对应碱基（或氨基酸）质量的文件格式。

- Fastq文件的一个read数据如下
  
  ```
  test@bioinfo_docker:~/mapping$ cat e_coli_1000_1.fq | head -n 4
  @r0/1
  TATTCTTCCGCATCCTTCATACTCCTGCCGGTCAG
  +
  EDCCCBAAAA@@@@?>===<;;9:99987776554
  ```
  
  - 第1行：以“@”开头，记录了read id，也可能是测序序列时的坐标和编号
  
  - 第2行：测序得到的

### Fasta (.fa)

## Sam文件

- SAM（The Sequence Alignment / Map format）格式，即序列比对文件的格式

- SAM文件由两部分组成，头部区和主体区，都以tab分列。头部区：以’@'开始，体现了比对的一些总体信息。比如比对的SAM格式版本，比对的参考序列，比对使用的软件等。主体区：比对结果，每一个比对结果是一行，有11个主列和一个可选列。

### 头部区简要介绍

1. `@HD VN:1.0 SO:unsorted`（排序类型）
   
   - VN是格式版本
   - SO表示对比序列的类型，有unknown（默认）、unsorted、queryname、coordinate几种

2. `@SQ SN:chrI LN:230218`（序列ID及长度）
   
   - 参考序列名，这些参考序列决定了比对结果sort的顺序
   
   - SN是参考序列名
   
   - LN是参考序列长度

3. `RG ID:sample01`（样品基本信息）
   
   - ID：样品的ID号
   
   - SM：样品名
   
   - LB：文库名
   
   - PU：测序
   
   - PL：测序平台

4. `@PG ID:Bowtie VN:1.0.0 CL:"bowtie -v 2 -m 10 --best --strata BowtieIndex/YeastGenome -f THA2.fa -S THA2.sam"`（比对所使用的软件及版本）
   
   - CL是运行程序

### 主体区简要介绍
