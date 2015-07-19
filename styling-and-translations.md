# Styling and Translations

样式和翻译

打包了 SkeletonApplication 的样式，但是我们需要改变标题和移除版权信息。

We’ve picked up the  SkeletonApplication’s styling, which is fine, but we need to change the title and remove the copyright message.

ZendSkeletonApplication 使用 `Zend\I18n` 的翻译功能来处理所有文字。它使用 `.po` 文件，文件存在 `module/Application/language`，你可以使用 [poedit](http://www.poedit.net/download.php) 来进行编辑。用 poedit 打开 `module/Application/language/en_US.po`。在 `Original` 设置列表中点击 Skeleton Application，然后输入 “Tutorial” 作为翻译的类型。

The ZendSkeletonApplication is set up to use `Zend\I18n` ’s translation functionality for all the text. It uses .po files that live in `module/Application/language`, and you need to use [poedit](http://www.poedit.net/download.php) to change the text. Start poedit and open `module/Application/language/en_US.po`. Click on “Skeleton Application” in the list of `Original` strings and then type in “Tutorial” as the translation.

![image](images/stylingandtranslations1.png)

点击工具栏的保存按钮，poedit 将会创建一个 `en_US.mo` 文件。如果你不能找到 `.mo` 文件的话，点击 `Preferences -> Editor -> Behavior` 然后选中复选框 `Automatically compile .mo file on save`。

Press Save in the toolbar and poedit will create an `en_US.mo` file for us. If you find that no .mo file is generated, check `Preferences -> Editor -> Behavior` and see if the checkbox marked `Automatically compile .mo file on save` is checked.

移除版权信息，我们需要编辑 `Application` 模块的 `layout.phtml` 视图脚本。

To remove the copyright message, we need to edit the `Application` module’s `layout.phtml` view script:

```php
 // module/Application/view/layout/layout.phtml:
 // Remove this line:
 <p>&copy; 2005 - 2014 by Zend Technologies Ltd. <?php echo $this->translate('All
 rights reserved.') ?></p>
```

现在这个页面看起来比之前苗条多了。

The page now looks ever so slightly better now!

![](images/stylingandtranslations2.png)
