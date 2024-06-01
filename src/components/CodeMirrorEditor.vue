<template>
    <div>
        <div ref="editor" class="editor"></div>
        <button @click="runCode">Run Code</button>
    </div>
</template>

<script>
    import { EditorState } from '@codemirror/state';
    //import { EditorView } from '@codemirror/basic-setup';
    import {basicSetup, EditorView} from "codemirror";
    import { javascript } from '@codemirror/lang-javascript';
    import { lineNumbers } from '@codemirror/view';

    export default {
        name: 'CodeMirrorEditor',
        data() {
            return {
                editorView: null,
                content: "let abc = 0"
            };
        },
        mounted() {
            this.initializeEditor();
        },
        methods: {
            initializeEditor() {
                const state = EditorState.create({
                    doc: this.content,
                    extensions: [basicSetup, javascript(), lineNumbers()]
                });

                this.editorView = new EditorView({
                    state,
                    parent: this.$refs.editor
                });
            },
            runCode() {
                const code = this.editorView.state.doc.toString();
                this.$emit('run-code', code);
            }
        },
        beforeUnmount() {
            if (this.editorView) {
                this.editorView.destroy();
            }
        }
    };
</script>

<style>
    .editor {
        border: 1px solid #ddd;
        height: 400px;
    }
</style>
