---
title: 设置仓库参与者指南
intro: 您可以创建告知人们应如何参与您的项目的指南。
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
redirect_from:
  - /articles/how-do-i-set-up-guidelines-for-contributors
  - /articles/setting-guidelines-for-repository-contributors
  - /github/building-a-strong-community/setting-guidelines-for-repository-contributors
topics:
  - Community
shortTitle: 参与指南
---

## 关于参与指南
为帮助项目参与者做好工作，您可以将含有参与指南的文件添加到项目仓库的根目录 `docs` 或 `.github` 文件夹。 有人打开拉取请求或创建议题时，他们将看到指向该文件的链接。 参与指南的链接也会出现在仓库的`贡献`页。 有关`贡献`页面的示例，请参阅 [github/docs/contribute](https://github.com/github/docs/contribute)。

![参与指南](/assets/images/help/pull_requests/contributing-guidelines.png)

对于仓库所有者，参与指南是告知人们应如何参与的一种途径。

对于参与者，该指南帮助他们确认其提交格式规范的拉取请求和打开有用的议题。

对于所有者和参与者来说，参与指南节省了由于不正确创建必须拒绝和重新提交的拉取请求或议题而导致的时间和麻烦。

{% ifversion fpt or ghes or ghec %}

您可以为组织{% ifversion fpt or ghes or ghec %} 或个人帐户{% endif %} 创建默认的参与指南。 更多信息请参阅“[创建默认社区健康文件](//communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)”。

{% endif %}

{% tip %}

**提示：**仓库维护员可以通过为仓库创建议题或拉取请求模板来设置议题的特定指南。 更多信息请参阅“[关于议题和拉取请求模板](/articles/about-issue-and-pull-request-templates)”。

{% endtip %}

## 添加 *CONTRIBUTING* 文件

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.files.add-file %}
3. 决定是在仓库的根目录 `docs` 还是 `.github` 目录中存储您的参与指南。 然后，在文件名字段中，输入文件的名称和扩展名。 参与指南文件名不区分大小写。 如果文件扩展名为支持的格式，文件会以富文本格式呈现。 更多信息请参阅“[使用非代码文件](/repositories/working-with-files/using-files/working-with-non-code-files#rendering-differences-in-prose-documents)”。 ![新文件名](/assets/images/help/repository/new-file-name.png)
    - 要使您的参与指南在仓库的根目录中显示，请输入 *CONTRIBUTING*。
    - 要使您的参与指南在仓库的 `docs` 目录中显示，请输入 *docs/* 以创建新目录，然后再输入 *CONTRIBUTING*。
    - 如果仓库包含多个 *CONTRIBUTING* 文件，则按以下顺序从位置中选择链接中显示的文件：`.github` 目录，然后是仓库的根目录，最后是 `docs` 目录。
4. 在新文件中，添加参与指南。 这些可能包括：
    - 创建良好议题或拉取请求的步骤。
    - 指向外部文档、邮件列表或行为准则的链接。
    - 社区和行为预期。
{% data reusables.files.write_commit_message %}
{% data reusables.files.choose_commit_branch %}
{% data reusables.files.propose_new_file %}

## 参与指南示例

如果您觉得难以着手，以下是参与指南的一些良好示例：

- Atom 编辑器[参与指南](https://github.com/atom/atom/blob/master/CONTRIBUTING.md)。
- Ruby on Rails [参与指南](https://github.com/rails/rails/blob/main/CONTRIBUTING.md)。
- Open Government [参与指南](https://github.com/opengovernment/opengovernment/blob/master/CONTRIBUTING.md)。

## 延伸阅读
- 开源指南的“[启动开源项目](https://opensource.guide/starting-a-project/)”部分{% ifversion fpt or ghec %}
- [{% data variables.product.prodname_learning %}]({% data variables.product.prodname_learning_link %}){% endif %}{% ifversion fpt or ghes or ghec %}
- "[添加许可到仓库](/articles/adding-a-license-to-a-repository)"{% endif %}
