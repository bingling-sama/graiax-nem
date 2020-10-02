# 快速上手

## 前言

!> 我们假设你已按照 mirai & Graia Framework 的文档正确地进行了配置

!> **注意！** 如果你在使用过程中出现错误，应先检查是否真的为 `graiax-nem` 的错误。如果是，请在我们的 [GitHub Issues](https://github.com/jinzhijie/graiax-nem/issues) 处报告你遇到的错误，我们会尽快处理。如果不是，请去相应的项目报告问题。

## 安装

请查看此处 → [安装](README.md#安装)

## 使用

```python:14,22,23
# bot.py
import asyncio

from graia.application import GraiaMiraiApplication, Session
from graia.application.event.messages import GroupMessage
from graia.application.group import Group
from graia.application.message.chain import MessageChain
from graia.application.message.elements.internal import Plain
from graia.broadcast import Broadcast

from graiax.nem import NEM
from graiax.nem.filters import Filters

BOTQQ = 10102333

loop = asyncio.get_event_loop()

bcc = Broadcast(loop=loop)
app = GraiaMiraiApplication(
    broadcast=bcc,
    connect_info=Session(
        host="http://localhost:8080",
        authKey="mirai-api-http-auth-key",
        account=BOTQQ,
        websocket=True
    )
)


@bcc.receiver(GroupMessage, headless_decoraters=[Filters.onAt(BOTQQ)])
async def nem_example(app: GraiaMiraiApplication, group: Group, _gm: GroupMessage):
    nem = NEM(_gm)
    await app.sendGroupMessage(group, MessageChain.create([Plain('YES!')]), quote=nem.source)


app.launch_blocking()
```

?> 别忘了修改代码中高亮行的具体值

运行代码，终端输出：

```log
[2020-09-26 15:47:02,929][INFO]: launching app...
[2020-09-26 15:47:02,960][INFO]: using websocket to receive event
[2020-09-26 15:47:02,964][INFO]: event reveiver running...
```

现在你可以在群里 @ 你的 bot 来测试。如果机器人回复了 `YES!` 那么恭喜你，你可以继续阅读下去了。

<!-- 
<script>
    new Vue({
        props: {
            nickname: String,
            color: String,
            avatar: String,
        },
        data: () => ({
            shown: false,
            active: false,
            moving: false,
        }),
        watch: {
            active (value) {
            if (!value) return this.shown = false
            const prev = this.$el.previousElementSibling && this.$el.previousElementSibling.__vue__
            if (prev && (prev.moving || !prev.shown)) {
                prev.$once('appear', this.appear)
            } else {
                this.appear()
            }
            },
        },
        mounted () {
            this.handleScroll()
            addEventListener('scroll', this.handleScroll)
            addEventListener('resize', this.handleScroll)
        },
        beforeDestroy () {
            removeEventListener('scroll', this.handleScroll)
            removeEventListener('resize', this.handleScroll)
        },
        methods: {
            appear () {
            this.shown = true
            this.moving = true
            setTimeout(() => {
                this.moving = false
                this.$emit('appear')
            }, 100)
            },
            handleScroll () {
            const rect = this.$el.getBoundingClientRect()
            if (rect.top < innerHeight) this.active = true
            },
        },
        }
    })
</script>


<style lang="stylus">
.chat-message
  position relative
  margin 1rem 0
  opacity 0
  transform translateX(-20%)
  transition transform 0.3s ease-out, opacity 0.3s ease
  &.shown
    opacity 1
    transform translateX(0)
.avatar
  width 3rem
  height 3rem
  position absolute
  border-radius 100%
  transform translateY(-1px)
  user-select none
  pointer-events none
  text-align center
  line-height 3rem
  font-size 1.6rem
  color white
  font-family "Comic Sans MS"
.nickname
  position relative
  margin 0 0 0.5rem 4.4rem
  font-weight bold
.message-box
  position relative
  margin-left 4.4rem
  width fit-content
  line-height 1.6
  border-radius 0.5rem
  background-color white
  word-break break-all
  .chat-message:not(.no-padding) &
    padding 0.6rem 0.8rem
  > img
    border-radius 0.5rem
  img
    vertical-align middle
  p > img
    margin 0.2rem 0
  &::before
    content ''
    position absolute
    right 100%
    top 0px
    width 12px
    height 12px
    border 0 solid transparent
    border-bottom-width 8px
    border-bottom-color currentColor
    border-radius 0 0 0 32px
    color white
  p
    margin 0
    line-height 1.6
</style> -->