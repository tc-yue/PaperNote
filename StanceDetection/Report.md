### SemEval-2016 Task6:Detecting Stance in Tweets
- 检测tweets中的立场：给定一篇推文和target 实体，判定作者是否支持这个给定的target，或者反对，或者中立。Target of interest 可能没有在推文中提到，系统明显发现，对于推断target的情感明显比推断其他实体的意见更难。
- Target可以是person，organization，goverment policy，movement，product。
    - Target:legalization of abortion
    - Tweet:The pregnant are more than walking incubators,and have rights!

- 中立类 无法推断文章的立场，缺乏明显的支持或者反对。标注时将除了favor和against的类别分类到neither
    - target：Hillary Clinton
    - Hillary Clinton has some strengths and some weaknesses
- Stance和Sentiment的区别
    - Sentiment：Determin whether a piece of text is postive,negative,or neutral.determin from text the speaker's opinion and the target of the opinion(the entity towards which opinion is expressed)
    - Stance determine favorability towards a given(pre-chosen)target of interest.The target of interest may not be explicity mentioned in the text and it may not be the target of opinion in the text.
        - Target:Donald Trump
        - Tweet:Jeb Bush is the only sane candidate in this republication lineup
    - Target of opininion  is JebBush, given target of interst is DonaldTrump.然而我们可以推断这篇文章作者是不支持DonaldTrump的。
    - 数据集既包含target of opinion and  Sentiment,stance towards a given target.
- 数据集标注方法
    -  需要包含了表达对target的意见，但没有直接包含这个词的句子
    -  需要包含target of opinion 和 target of interest 不同的句子