<template>
    <div>
        <div v-html="converted" class="output"></div>
        <textarea v-model="msg"></textarea>
    </div>
</template>
<style>
    .emoji {
        vertical-align: middle;
        height: 22px;
        cursor: pointer;
    }


    .output {
        height: 22rem;
        width: 30rem;
        white-space: pre-wrap;
    }


    textarea {
        height: 22rem;
        width: 30rem;
    }
</style>

<script>
    function replaceNth(src, search, index, rep){
        var nth = 0;
        return src.replace(new RegExp(search, "g"), function (match, i, original) {
            console.log(search, nth)
            nth++;
            return (nth === index) ? rep : match;
        });
    }
    import * as Vue from "vue"
    import * as emoji from "node-emoji"

    function match(input, emojiName){
        return input.indexOf(emoji.get(emojiName)) >= 0
    }


    function injectSpanItem(input, name){
        return input.replace(new RegExp(emoji.get(name), 'g'), `<span class='emoji emoji-${name}'>${emoji.get(name)}</span>`)
    }
    function injectSpan(input){
        input = injectSpanItem(input, "apple")
        input = injectSpanItem(input, "pineapple")
        input = injectSpanItem(input, "white_medium_square")
        input = injectSpanItem(input, "white_check_mark")
        input = injectSpanItem(input, "moneybag")
        input = injectSpanItem(input, "fire")
        return input
    }


    export default {
        name: 'app',
        data() {
            return {
                msg: '',
                converted: ""
            }
        },
        watch: {
            "msg": function () {
                this.converted = emoji.emojify(this.msg)
                this.converted = injectSpan(this.converted)

                Vue.nextTick(() => {
                    //イベントの追加
                    //apple -> pineapple
                    var emojis = document.querySelectorAll('.emoji-apple');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":apple:", i + 1, ":pineapple:")
                        });
                    }
                    //pineapple -> apple
                    emojis = document.querySelectorAll('.emoji-pineapple');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":pineapple:", i + 1, ":apple:")
                        });
                    }

                    emojis = document.querySelectorAll('.emoji-white_medium_square');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":white_medium_square:", i + 1, ":white_check_mark:")
                        });
                    }

                    emojis = document.querySelectorAll('.emoji-white_check_mark');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":white_check_mark:", i + 1 , ":white_medium_square:")
                        });
                    }

                    emojis = document.querySelectorAll('.emoji-moneybag');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":moneybag:", i + 1 , ":moneybag::moneybag:")
                        });
                    }

                    emojis = document.querySelectorAll('.emoji-fire');
                    for (let i = 0; i < emojis.length; i++) {
                        emojis[i].addEventListener('click', (event)=> {
                            this.msg = replaceNth(this.msg, ":fire:", i + 1 , "")
                        });
                    }
                    
                })
            }
        },
        mounted: function () {
            this.msg = "Click Emojis!\n:apple: :apple: :apple:\n\n:white_medium_square: TODO\n:white_check_mark: TODO2\n:white_check_mark: TODO3 :fire::fire::fire: \n\n:moneybag:"
        }
    }

</script>