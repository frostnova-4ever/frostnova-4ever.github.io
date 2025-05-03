<template>
    <div>
        <editor :init="editorConfig" v-model="editorContent"></editor>
        <button @click="getContent">获取内容</button>
        <button @click="setContent">设置内容</button>
    </div>
</template>

<script>
import Editor from '@tinymce/tinymce-vue';

export default {
    components: {
        Editor
    },
    data() {
        return {
            editorContent: '',
            editorConfig: {
                height: 500,
                menubar: false,
                plugins: [
                    'advlist autolink lists link image charmap print preview anchor',
                    'searchreplace visualblocks code fullscreen',
                    'insertdatetime media table paste code help wordcount'
                ],
                toolbar: 'undo redo | formatselect | ' +
                    'bold italic backcolor | alignleft aligncenter ' +
                    'alignright alignjustify | bullist numlist outdent indent | ' +
                    'removeformat | help'
            }
        };
    },
    methods: {
        getContent() {
            console.log(this.editorContent);
        },
        setContent() {
            this.editorContent = '<p>这是设置的内容。</p>';
        }
    }
};
</script>