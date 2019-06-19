==数据预处理==
## 英文Twitter
[twitter 情感分析](https://github.com/abdulfatir/twitter-sentiment-analysis/tree/master/docs)

---------
- 小写化
- 将2各以上的.换成空格
- strip 引号和空格
- 替换2个以上的空格成一个
- 使用正则表达式((www\.[\S]+)|(https?://[\S]+))替换url
- 使用正则表达式@[\S]+替换USER_MENTION
- 使用EMO_POS 或 EMO_NEG 替换表情符
- 符号# 替换
- url替换 表情符替换、重复字符替换sooooo 替换成soo、小写化
- Tokenizer:避免将一个token拆开，识别emoticons。emoji，dates，times。spell correction，word normalization. Normailize urls usermention http