# Missing-Semester 字幕工作

本文档描述了 Missing-Semester 字幕校对的工作流程和相关规范, 希望对以后的搬运工作有所帮助。为了保证字幕校对的高效性和一致性，我们采用网盘管理，而不使用 Git。同时，我制定了一套规范的命名系统，并使用 ARCTIME 软件来添加字幕。

> 为啥不用 git?首先, 我们的 subtitler 对于 git 的使用不熟练; 其次反复校对的视频文件很大, 使用 git 并不方便. 现在反思, 视频文件大就不传视频文件.

## 最终版字幕制作流程

我工作制定了许多套流程, 为了尽可能提高效率, 经过逐步的优化, 最终版如下.(lecture6 之后按照此流程制造)

### 文件夹结构

```
missing-semester->init_subtitles
                    ->en_subtitles
                        ->lecture01_en_ai_sub.srt
                        ->lecture02_en_ai_sub.srt
                        ....
                    ->ch_subtitles
                        ->lecture01_ch_ai_sub.srt
                        ->lecture02_ch_ai_sub.srt
                        ....
                ->校对
                    ->lecture01
                        ->lecture01_sub_modi00.srt
                        ->lecture01_sub_modi01.md
                        ->lecture01_sub_modi02.srt
                        ->lecture01_sub01.mp4
                        ->lecture01_sub02.mp4
                        ->questions01.md
                        ->questions02.md
                        ...
                    ->lecture02
                        ...
                ->成品
                    ->lecture01
                        ->完整名字.mp4
                        ->完整名字.srt
                    ->lecture2
                    ...
```

### 流程

```lua
function subtitle_pre-processing(en_ai_subtitles):
    断句 en_ai_subtitles | chatgpt | 文字处理(比如中文符号改为英文之类的)
        output: 断好句版 en_ai_subtitles
    翻译 en_ai_subtitles | chatgpt | 文字处理
        output: 断好句版 ch_ai_subtitles
    # 实际上,chatgpt的翻译质量并不好,也许是我的prompt的问题,但我已经尽力完善prompt了

function first:
    subtitler将ai英文,ai中文字幕添加到视频里:
        大致处理好视频中的字幕断句和时间轴.
        -- 为了提高效率, 这里不要压制成片
        output: lecturex_sub_modi00.srt, [questions]上传网盘
    translator从网盘下载lecturex_sub_modi00.srt, [questions]:
        直接看字幕文件翻译中文字幕(这里的翻译较为书面化)
        output:lecturex_sub_modi01.srt, [questions]
function second:
    subtitler将lecturex_sub_modi01.srt添加到视频里:
        根据questions里面修改建议进行修改,压制成片
        output: lecturex_sub_modi02.srt, lecturex_sub02.mp4, [questions]上传网盘
    translator从网盘下载lecturex_sub_modi00.srt, [questions]:
        校对英文字幕(这一遍尽量保证英文的准确无误)
        校对中文字幕(根据视频内容, 将较为书面化的翻译口语化)
        在校对过程中将 代码高亮,字幕遮挡,字幕旁批,时间轴错误,断句问题以及对应的解决方案 记录到questions
        output:lecturex_sub_modi03.srt, [ questions ]
function after_second:
    subtitler将lecturex_sub_modi03.srt添加到视频里:
        根据questions里面修改建议进行修改,压制成片
        output: lecturex_sub_modi02.srt, lecturex_sub02.mp4, [questions]上传网盘
    translator从网盘下载lecturex_sub_modi00.srt, [questions]:
        校对中英字幕
        在校对过程中将 代码高亮,字幕遮挡,字幕旁批,时间轴错误,断句问题以及对应的解决方案 记录到questions
        output:lecturex_sub_modi04.srt, [ questions ]
function substantial_modi_01:
    first
    second
    while(not generally satisfied): after_second


function minor_modi(lecturex_sub_modi[x].srt,questions):
    subtitler根据questions来调整字幕, 如有疑问, 在线提出协商解决.
        output: lecturex_sub_modi0[x+1].srt,lecturex_modi0[x+1].srt,[ questions ]
    translator从网盘下载lecturex_sub_modi02.srt,lecturex_modi0[x+1].srt, [questions]:
        观看视频, 直接在线交流提出修改意见
        压制成片


main:
    subtitle_pre-processing
    substantial_modi
    while(not satisfied){
        minor_modi
    }
```

## 涉及到的问题以及对应的解决方案:

- 断句:
  - 一般在标点处, and 处, 不同人说话的地方断句.
- 注释样式:
  - 英文字幕不更改样式.中文字幕斜体.
- 英文有时候会比中文长很多
  - 对英文换行处理
- 遮挡:
  - 如果字幕遮挡到命令首先选择上移字幕解决,如果上移后背景纷杂影响到字幕,则对字体加粗换显眼的颜色.
  - 如果未遮挡命令,而背景对字幕有扰乱效果,则直接原地加粗更换颜色.

### 字幕样式

- 普通中文: default_cn,幼圆,45,&H00FFFFFF,&H00FFFFFF,&H19000000,&H910E0807,-1,0,0,0,100.0,100.0,2.8788927,0.0,1,3.3076925,1.2,2,135,135,72,1
- 普通英文: default_en,Arial,36,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100.0,100.0,0.0,0.0,1,2.0,2.0,2,10,10,30,1
- 换行英文: default_en2,Arial,32,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100.0,100.0,0.0,0.0,1,2.0,2.0,2,10,10,10,1
- 中文中代码标识: code,Arial,45,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,-1,-1,0,0,100.0,100.0,0.0,0.0,1,2.0,2.0,2,10,10,40,1
- 中文字幕中注释: comment1,楷体,45,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,-1,0,0,100.0,100.0,0.0,0.0,1,2.0,2.0,2,10,10,40,1
- 注释本身: comment2,楷体,40,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100.0,100.0,0.0,0.0,1,1.5,2.0,2,10,10,135,1
- 上移中文: move_cn,幼圆,45,&H00FFFFFF,&H000000FF,&H00000000,&H910E0807,-1,0,0,0,100.0,100.0,0.0,0.0,1,3.3,1.2,2,10,10,145,1
- 上移英文: move_en,Arial,32,&H00FFFFFF,&H000000FF,&H00000000,&H00000000,0,0,0,0,100.0,100.0,0.0,0.0,1,2.0,2.0,2,10,10,110,1
- 看不清的中文（加蓝边框）: unclear_cn,微软雅黑,60,&H00FFFFFF,&H000000FF,&H00FF0004,&H910E0807,-1,0,0,0,100.0,100.0,0.0,0.0,1,1.0,1.2,2,10,10,145,1
- 看不清的英文（加蓝边框）: unclear_en,Arial,32,&H00FFFFFF,&H000000FF,&H00FF0004,&H00000000,-1,0,0,0,100.0,100.0,0.0,0.0,1,1.0,2.0,2,10,10,110,1

## 学到的一些

对于小的需求比较固定的项目来说,工作前需要精致的流程设计,尽可能地提高效率.(这里的字幕工作应该属于此类)
对于大的项目,需求比较多的项目来说,小步快跑,逐步优化也许更合适一些.(即所谓的 KISS 原则)

前期的懒惰不仔细,流程的不规范,标准的不统一,都会给后期的工作带来巨大的麻烦, 造成时间的大量浪费.

## 其他资源

- 官方 github 仓库:https://github.com/missing-semester/missing-semester
- 2020 讲义:https://missing.csail.mit.edu/
- 2020 中文讲义:https://missing-semester-cn.github.io/
- 2019 讲义:https://missing.csail.mit.edu/2019/

## 捐赠

如果您觉得这个工作对您有帮助, 如果可以的话, 奖励我一杯冰阔落吧!感谢您的慷慨与善意.
<img src="images/donate.jpg" alt="donate" style="width:50%;">
