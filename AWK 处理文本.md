AWK 处理文本

 awk '{print $1}' example1.txt

 awk -F, '{ print "Email"$1"xxx'\''xx"$2 }' test.csv

```
awk -F, '{ print "Crowdsale::Qualifier.create(key: '\''zcash_mining'\'', email: '\''"$1"'\'', amount: "$2", is_take: false)" }' zcash.csv > zcash_table.rb
```

输出成rb档案 可以直接使用





Crowdsale::Qualifier.create(key: 'zcash_mining', email: 'lixiaolai@gmail.com', amount: 36, is_take: false)
Crowdsale::Qualifier.create(key: 'zcash_mining', email: '871961699@qq.com', amount: 36, is_take: false)