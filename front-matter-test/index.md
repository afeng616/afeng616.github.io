# 这是一个Front Matter插件测试


# 问题
Front Matter插件通过`Contents`分栏中的`Create content`按钮生成的新文章，默认生成位置与之前不同（不在posts下），是否会影响最终文章生成？

另外需要思考的一点是，content下的目录又有何讲究，这值得再细究一下！

# 回答
将多个目录作为`Content folders`后，在点击`Create content`后是可以选择生成文件所在位置的。

若文章与`posts`目录同级，则无法被收录到`所有文章`列表中（不影响直接通过md名称访问该文章）。因此，还是将普通文章统一放在`posts`目录下为好。
