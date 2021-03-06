# HighlightedSearch

## 基本介绍
* 类似微信的客户端本地搜索、搜索关键字高亮显示

Demo 效果如图所示
![](https://github.com/PengfeiWang666/HighlightedSearch/blob/master/ReadMeImage/screenshots_1.gif)

支持：
* 汉字支持汉字直接搜索、拼音全拼搜索、拼音简拼搜索
* 搜索匹配到的关键字高亮显示
* 搜索结果优先显示全部匹配、其次是拼音全拼匹配、拼音简拼匹配；关键字在结果字符串中位置越靠前，优先显示
* 支持搜索英文、汉字、电话号码及混合搜索

* 英文名称及电话号码的搜索直接使用完全匹配的方式即可
* 重难点是汉字的拼音相关的拼音全拼、简拼搜索，比如 “刘亦菲” 对应的搜索关键字`有且只有`以下三大类总计 25 种匹配
  * 汉字：“刘”、“亦”、“菲”、“刘亦”、“亦菲”、“刘亦菲”
  * 简拼相关："l"、"y"、"f"、"ly"、"yf"、"lyf" 
  * 全拼相关："li"、"liu"、"liuy"、"liuyi"、"liuyif"、"liuyife"、"liuyifei"、"yi"、"yif"、"yife"、"yifei"、"fe"、"fei"
* 拼音的重难点还包括：比如搜索关键字为“xian”，既要匹配出“先”，也要匹配出“西安”

## [代码细节讲解](https://juejin.im/post/5a32212c6fb9a0452341e80c)


## 使用方法
##### 1. 事例工程

* git clone git@github.com:PengfeiWang666/HighlightedSearch.git
* cd Example
* open HighlightedSearch.xcworkspace

##### 2. Install

* pod "HighlightedSearch"

##### 3. Usage
```objc
// WPFPinYinDataManager 依次添加数据源（标识符为了防止重名现象）
+ (void)addInitializeString:(NSString *)string identifer:(NSString *)identifier

// 更新搜索结果
- (void)updateSearchResultsForSearchController:(UISearchController *)searchController {
    ...
    ...
    for (WPFPerson *person in [WPFPinYinDataManager getInitializedDataSource]) {
        WPFSearchResultModel *resultModel = [WPFPinYinTools searchEffectiveResultWithSearchString:keyWord Person:person];
        if (resultModel.highlightedRange.length) {
            person.highlightLoaction = resultModel.highlightedRange.location;
            person.textRange = resultModel.highlightedRange;
            person.matchType = resultModel.matchType;
                [resultDataSource addObject:person];
        }
}
```
