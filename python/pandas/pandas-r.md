# Pandas-R

source ENV/bin/activate  
jupyter notebook  
iat  
postion  
按行  
axis=1  
输出csv的时候不输出索引  
index=False  
创建  
s = pd.Series\(\[1,3,5,np.nan,6,8\]\)  
时间range  
dates = pd.date\_range\('20130101', periods=6\)  
dates为列 行为ABCD np random 6列4行的数据  
df = pd.DataFrame\(np.random.randn\(6,4\), index=dates, columns=list\('ABCD'\)\)  
查看df的列  
df.index  
查看df的行  
df.columns  
查看df的值  
df.values  
df的各种运算  
df.describe\(\)  
df的转梽  
df.T  
按照column id 排序 ascending=False为降序  
df.sort\_index\(axis=1, ascending=False\)  
取A列  
df\['A'\]  
取前3列  
df\[0:3\]  
取'20130102'到'20130104'  
df\['20130102':'20130104'\]  
取第一个date的那行数据  
df.loc\[dates\[0\]\]  
取A,B列的数据  
df.loc\[:,\['A','B'\]\]  
取02月到04月的A,B列数据  
df.loc\['20130102':'20130104',\['A','B'\]\]  
取02月A,B列数据  
df.loc\['20130102',\['A','B'\]\]  
取第一个月的A列数据  
df.loc\[dates\[0\],'A'\]  
取第4行的值  
df.iloc\[3\]  
取第4第5行，第1第2列  
df.iloc\[3:5,0:2\]  
取2，3，5行，第1第2列  
df.iloc\[\[1,2,4\],\[0,2\]\]  
取第2，3行  
df.iloc\[1:3,:\]  
取第2，3列  
df.iloc\[:,1:3\]  
取第2行第2列  
df.iloc\[1,1\]  
取A列大于0的行数据  
df\[df.A &gt; 0\]  
取大于0的所有数据  
df\[df &gt; 0\]  
df2复制于df  
df2 = df.copy\(\)  
给E列重新赋值  
df2\['E'\] = \['one', 'one','two','three','four','three'\]  
取E列里\['two','four'\]的所有行  
df2\[df2\['E'\].isin\(\['two','four'\]\)\]  
从20130102开始创建6列数据，六列对应的值是\[1,2,3,4,5,6\]  
s1 = pd.Series\(\[1,2,3,4,5,6\], index=pd.date\_range\('20130102', periods=6\)\)  
给df表的\[dates\[0\],'A'\]赋值为0  
df.at\[dates\[0\],'A'\] = 0  
df的F列为s1  
df\['F'\] = s1  
df的第3行第2列为0  
df.iat\[0,1\] = 0  
df的D列赋值为np.array\(\[5\]  _len\(df\)\) ! 把5输出6遍  
df.loc\[:,'D'\] = np.array\(\[5\]_  len\(df\)\)  
df2中大于0的取反 这样df2的大于0的值全为负  
df2\[df2 &gt; 0\] = -df2  
生成一个新的df1的表，第二行不为null  
df1 = df.reindex\(index=dates\[0:4\], columns=list\(df.columns\) + \['E'\]\)  
df1.loc\[dates\[0\]:dates\[1\],'E'\] = 1  
去除df1中含null的行  
df1.dropna\(how='any'\)  
把df1中null的值赋值为5  
df1.fillna\(value=5\)  
df1中如果是null的话 这个值为True,其余为False  
pd.isnull\(df1\)  
求df每一列的平均数  
df.mean\(\)  
求第一行的平均数null不算  
df.mean\(axis=1\)  
对df的A列全部加1  
df\['A'\] = df\['A'\].apply\(lambda x:x+1\)  
对value的值进行统计  
s = pd.Series\(np.random.randint\(0, 7, size=10\)\)  
s.value\_counts\(\)  
把s表中的value大学转化为小写  
s = pd.Series\(\['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'\]\)  
s.str.lower\(\)  
把一个数组的所有的DataFrame拼接成一个大的DataFrame  
BORM = pd.concat\(results\)  
left 和 right 按照key列为主键进行合并  
left = pd.DataFrame\({'key': \['foo', 'foo'\], 'lval': \[1, 2\]}\)  
right = pd.DataFrame\({'key': \['foo', 'foo'\], 'rval': \[4, 5\]}\)  
pd.merge\(left, right, on='key'\)  
df后面加上s数列\(s为一个DataFrame\)  
df = pd.DataFrame\(np.random.randn\(8, 4\), columns=\['A','B','C','D'\]\)  
s = df.iloc\[3\]  
df.append\(s, ignore\_index=True\)  
根据df的列A,B进行groupby cd列数字求和  
df = pd.DataFrame\({'A' : \['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'\],  
'B' : \['one', 'one', 'two', 'three','two', 'two', 'one', 'three'\],  
'C' : np.random.randn\(8\),  
'D' : np.random.randn\(8\)}\)  
df.groupby\(\['A','B'\]\).sum\(\)  
tuples = list\(zip\(_\[\['bar', 'bar', 'baz', 'baz',  
'foo', 'foo', 'qux', 'qux'\],  
\['one', 'two', 'one', 'two',  
'one', 'two', 'one', 'two'\]\]\)\)  
index = pd.MultiIndex.from\_tuples\(tuples, names=\['first', 'second'\]\)  
df = pd.DataFrame\(np.random.randn\(8, 2\), index=index, columns=\['A', 'B'\]\)  
生产n_m全零矩阵  
np.zeros\(\(n,m\)\)

