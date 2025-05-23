# hajimi 
### 由Magic Beach编写

- 本项目目前分为两部分[studio轮询][vertex模式]，两部分互相独立
- 本项目可在主流云平台“huggingface/claw”、vps、Windows本地一键端进行部署

## Gemini主流白嫖平台
- AI studio（Google账号注册）
  - 目前免费：仅flash模型
- vertex AI（绑卡）
  - 目前免费：仅0325pro-exp（每分钟上下文131k）
- vertex AI express（快速模式，无需绑卡，点击领取，有些人能领有些人不能）
  - 只能使用：0506pro-pre，flash0520（0325pre=0506pre，0417flash=0520flash）

## 本项目支持
- 轮询模式：使用AI studio key
  - 除密码外，变量全部是轮询模式使用，vertex模式无效
- vertex模式：使用vertex AI，vertex AI express
  - vertex AI需要json文件，vertex AI express只需要填写key
  
  ## !! 不同平台不同模型的审核、报错不同，不同部署方式出问题的情况也不同，因此相互之间比较没有任何意义！
  - 问报错之前请先提供：
    - 项目在哪里？或用什么部署的
	- 你使用的Gemini平台是哪个？
	- 你使用的Gemini模型是哪个？
	- 项目前端的“系统日志”截图

# huggingface部署教程（轮询部分）
- 完全部署你需要经历[在github构建镜像]→[在huggingface创建空间]→[在酒馆中连通]

# 0. 在github构建镜像
（⬇️⬇️⬇️以下操作全部在github网站进行）

## 0.1 Fork本项目
- 点击链接：[https://github.com/wyeeeee/hajimi/fork]
- 填写`Repository name`
- 点击底部绿色按钮`Create fork`完成Fork操作

## 0.2 构建镜像
- 点击顶部的`Action`
- 点击绿色按钮`I understand my workflows, go ahead and enable them`
- 在左侧侧边栏点击`GHCR CI`
- 点击右侧的`Run workflow`按钮
- 直接点击弹出的`Run workflow`开始构建镜像（需要等待一些时间）
⚠️镜像地址为：ghcr.io/你的github用户名/hajimi:latest
  - 例如：ghcr.io/wyeeeee/hajimi:latest。
  - 记住这个镜像等下要在huggingface中填写


# 1. 在huggingface创建空间
（⬇️⬇️⬇️以下操作全部在huggingface网站进行）

## 1.1 前置作业
- 注册huggingface：[https://huggingface.co/]

## 1.2 创建Spaces (空间)
- 进入huggingface创建Spaces页面：[https://huggingface.co/spaces]
- 在右上角点击`+New Spaces`
- 必填/必选项：
  - Owner（默认是你的用户名，别动）
  - space name（自己填写，根据网站判断会出现各种错误，比如重名、有大写，自己填一个可用的）
  - Select the Space SDK（选择“Docker”，点开后用默认的“Blank”即可，不要选别的）
  - 最后**一定**要选择`public`
  - 点击`Create Space`，完成空间创建

## 1.3 部署本项目
- 空间创建好之后点击顶部的`Files`
- 点击右上角`Contribute`，选择`Create a new file`
⚠️接下来是重点不要填错！
- 在“Name your file”输入框填写：**！！只能填写`Dockerfile`这几个字！！**
- 在"Edit"中粘贴以下内容
```
FROM ghcr.io/这里填你的github用户名/hajimi:latest

EXPOSE 7860

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "7860"]
```
*注意：“ghcr.io/这里填你的github用户名/hajimi:latest”就是之前说的你的镜像，如果github用户名有大写是不识别的，改为小写*

- 全部填写完毕其他位置不用动，直接点击最底部`Commit new file to main`，完成部署

## 1.4 但还没完，配置必须项（变量）
- 点击顶端`settings`
- 下拉找到`Variables and secrets`
- 点击`New secrets`

必须变量：
- Name填写`PASSWORD` | value填写`你的密码` (你不设置默认密码是123) | 点击保存
- Name填写`GEMINI_API_KEYS`|  value填写`你的key` (多个key需要用“,”英文逗号隔开，不能换行) | 点击保存<br>
`⛔️免责声明：key是你的个人资产，为了安全隐私，因此保存后变量中不会显示你输入过的key，！！请一定保存好！！`<br>
💡更多变量，点击下载txt：[https://github.com/wyeeeee/hajimi/releases/tag/settings]


# vertex模式通用教程
（⬇️⬇️⬇️以下内容在项目前端页面操作）
首先需要你走完“huggingface部署教程（轮询部分）”（或者在其他平台部署好，原理都是一样的）
- 点击右上角打开`vertex`
- 填写`Vertex 配置`
- 分支
  - 如果你是vertex绑卡用户：直接填写`Google Credentials JSON`和你的密码，点击保存（不要打开`Vertex Express`）
  - 如果你是vertex快速模式用户：打开`Vertex Express`，在`Vertex Express API密钥`填写你的AQ密钥和你的密码，点击保存（不要碰`Google Credentials JSON`你没有也不需要）


# 项目更新
1.每次更新项目时，需要先回到github，对你Fork的项目进行更新
  - 在github右上角你的头像，进入你的个人主页就可以看到你Fork过的所有项目
  - 在里面找到hajimi并点击
  - 点击后上方会有`Sync fork`按钮，点击
  - 弹出界面点击绿色按钮`Update branch`进行更新<br>
2.进入huggingface空间，进行更新（就是本项目的前端界面）
  - 点击顶部`Files`，选择后面的“⁝”符号，点击`Restart Space`（理论上可以，不行看3）<br>
3.另一种更新获取
  - 如果上面方式无效点击`Dockerfile`这个文件名，进入
  - 点击`Edit`
  - 把“FROM ghcr.io/jairo-t/hajimi:latest”这句结尾的“latest”改为最新版本号，比如0.3.1
  - 点击底部`Commit changes to main`
*注意：无论用2还是3，都要先在github更新你Fork的项目*

# 在酒馆中连通
- 选择`自定义（兼容OpenAI）`
- 填写链接与密钥：
  - huggingface链接：`https://你的huggingface用户名-你的空间名.hf.space/v1`，示例：https://tt335-hiiijimi.hf.space/v1
  - 密钥：你在1.4配置的`PASSWORD`