---
title: 转让仓库
intro: 您可以将仓库转让给其他用户或组织帐户。
redirect_from:
  - /articles/about-repository-transfers
  - /move-a-repo
  - /moving-a-repo
  - /articles/what-is-transferred-with-a-repository
  - /articles/what-is-transferred-with-a-repo
  - /articles/how-to-transfer-a-repo
  - /articles/how-to-transfer-a-repository
  - /articles/transferring-a-repository-owned-by-your-personal-account
  - /articles/transferring-a-repository-owned-by-your-organization
  - /articles/transferring-a-repository
  - /github/administering-a-repository/transferring-a-repository
  - /github/administering-a-repository/managing-repository-settings/transferring-a-repository
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Repositories
---

## 关于仓库转让

当您将仓库转让给新所有者时，他们可以立即管理仓库的内容、议题、拉取请求、版本、项目板和设置。

存储库传输的先决条件：
- When you transfer a repository that you own to another user account, the new owner will receive a confirmation email.{% ifversion fpt or ghec %} The confirmation email includes instructions for accepting the transfer. 如果新所有者在一天之内没有接受转让，则邀请将过期。{% endif %}
- 要将您拥有的仓库转让给一个组织，您必须拥有在目标组织中创建仓库的权限。
- 目标帐户不得具有相同名称的仓库，或位于同一网络中的复刻。
- 仓库原来的所有者将添加为已转让仓库的协作者。 Other collaborators to the transferred repository remain intact.{% ifversion ghec or ghes or ghae %}
- Internal repositories can't be transferred.{% endif %}
- 私有复刻无法进行转让。

{% ifversion fpt or ghec %}如果您将私有仓库转让给 {% data variables.product.prodname_free_user %} 用户或组织帐户，该仓库将无法访问比如受保护分支和 {% data variables.product.prodname_pages %} 之类的功能。 {% data reusables.gated-features.more-info %}{% endif %}

### 随仓库一起转让的有哪些内容？

当您转让仓库时，其议题、拉取请求、wiki、星号和查看者也将转让。 如果转让的仓库包含 web 挂钩、服务、密码或部署密钥，它们将在转让完成后保持关联状态。 关于提交的 Git 信息（包括贡献）都将保留。 此外：

- 如果转让的仓库是复刻，则它仍与上游仓库关联。
- 如果转让的仓库有任何复刻，则这些复刻在转让完成后仍与该仓库关联。
- 如果转让的仓库使用 {% data variables.large_files.product_name_long %}，则所有 {% data variables.large_files.product_name_short %} 对象均自动移动。 This transfer occurs in the background, so if you have a large number of {% data variables.large_files.product_name_short %} objects or if the {% data variables.large_files.product_name_short %} objects themselves are large, it may take some time for the transfer to occur.{% ifversion fpt or ghec %} Before you transfer a repository that uses {% data variables.large_files.product_name_short %}, make sure the receiving account has enough data packs to store the {% data variables.large_files.product_name_short %} objects you'll be moving over. 有关为用户帐户增加存储的更多详细，请参阅“[升级 {% data variables.large_files.product_name_long %}](/articles/upgrading-git-large-file-storage)”。{% endif %}
- 仓库在两个用户帐户之间转让时，议题分配保持不变。 当您将仓库从用户帐户转让给组织时，分配给组织中该成员的议题保持不变，所有其他议题受理人都将被清除。 只允许组织中的所有者创建新的议题分配。 当您将仓库从组织转让给用户帐户时，只有分配给仓库所有者的议题保留，所有其他议题受理人都将被清除。
- 如果转让的仓库包含 {% data variables.product.prodname_pages %} 站点，则指向 Web 上 Git 仓库和通过 Git 活动的链接将重定向。 不过，我们不会重定向与仓库关联的 {% data variables.product.prodname_pages %}。
- 指向以前仓库位置的所有链接均自动重定向到新位置。 当您对转让的仓库使用 `git clone`、`git fetch` 或 `git push` 时，这些命令将重定向到新仓库位置或 URL。 不过，为了避免混淆，我们强烈建议将任何现有的本地克隆副本更新为指向新仓库 URL。 您可以通过在命令行中使用 `git remote` 来执行此操作：

  ```shell
  $ git remote set-url origin <em>new_url</em>
  ```

- When you transfer a repository from an organization to a user account, the repository's read-only collaborators will not be transferred. This is because collaborators can't have read-only access to repositories owned by a user account. For more information about repository permission levels, see "[Permission levels for a user account repository](/github/setting-up-and-managing-your-github-user-account/permission-levels-for-a-user-account-repository)" and "[Repository roles for an organization](/organizations/managing-access-to-your-organizations-repositories/repository-roles-for-an-organization)."{% ifversion fpt or ghec %}
- Sponsors who have access to the repository through a sponsorship tier may be affected. For more information, see "[Adding a repository to a sponsorship tier](/sponsors/receiving-sponsorships-through-github-sponsors/managing-your-sponsorship-tiers#adding-a-repository-to-a-sponsorship-tier)".{% endif %}

更多信息请参阅“[管理远程仓库](/github/getting-started-with-github/managing-remote-repositories)”。

### 仓库转让和组织

要将仓库转让给组织，您必须在接收组织中具有仓库创建权限。 如果组织所有者已禁止组织成员创建仓库，则只有组织所有者能够转让仓库出入组织。

将仓库转让给组织后，组织的默认仓库权限设置和默认成员资格权限将应用到转让的仓库。

## 转让您的用户帐户拥有的仓库

您可以将仓库转让给接受仓库转让的任何用户帐户。 在两个用户帐户之间转让仓库时，原来的仓库所有者和协作者将自动添加为新仓库的协作者。

{% ifversion fpt or ghec %}If you published a {% data variables.product.prodname_pages %} site in a private repository and added a custom domain, before transferring the repository, you may want to remove or update your DNS records to avoid the risk of a domain takeover. 更多信息请参阅“[管理 {% data variables.product.prodname_pages %} 网站的自定义域](/articles/managing-a-custom-domain-for-your-github-pages-site)。{% endif %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.transfer-repository-steps %}

## 转让您的组织拥有的仓库

如果您具有组织中的所有者权限或组织其中一个仓库的管理员权限，您可以将组织拥有的仓库转让给用户帐户或其他组织。

1. 在拥有仓库的组织中登录您具有管理员或所有者权限的用户帐户。
{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.transfer-repository-steps %}
