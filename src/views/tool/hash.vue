<template>
    <div>
        <Input v-model="current.input" :rows="7" type="textarea" placeholder="内容"></Input>
        <option-block>
            <FormItem>
                <ButtonGroup>
                    <Button type="primary" @click="handle(v)" v-for="v in type" :key="v">{{ v }}</Button>
                </ButtonGroup>
            </FormItem>
            <FormItem>
                <Checkbox v-model="current.isUppercase">大写字母</Checkbox>
            </FormItem>
        </option-block>
        <Input v-model="current.output" :rows="7" type="textarea" placeholder="结果"></Input>
    </div>
</template>
<script>
import crypto from "crypto-js"

export default {
    created() {
        this.current = Object.assign(this.current, this.$getToolData("input"))
    },
    methods: {
        handle(v) {
            if (this.current.input) {
                switch (v) {
                    case "md5":
                        this.current.output = crypto.MD5(this.current.input).toString();
                        break;
                    case "sha1":
                        this.current.output = crypto.SHA1(this.current.input).toString();
                        break;
                    case "sha256":
                        this.current.output = crypto.SHA256(this.current.input).toString();
                        break;
                    case "sha512":
                        this.current.output = crypto.SHA512(this.current.input).toString();
                        break;
                    case "sm3":
                        this.current.output = require('sm-crypto').sm3(this.current.input);
                        break;
                    default:
                        return;
                }
                if (this.current.isUppercase) {
                    this.current.output = this.current.output.toUpperCase()
                }
                this.current.operation = v;
                this.$clipboardCopy(this.current.output);
                this.$saveToolData(this.current);
            }
        }
    },
    data() {
        return {
            current: {
                input: "",
                isUppercase: false,
                output: "",
                operation: ""
            },
            type: ['md5', 'sha1', 'sha256', 'sha512', "sm3"]
        }
    },
}
</script>