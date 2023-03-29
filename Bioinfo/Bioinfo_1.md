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

## Fastq文件

- Fastq格式是一种基于文本的存储生物序列和对应碱基（或氨基酸）质量的文件格式。
