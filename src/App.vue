<template>
    <div id="app">
        <DocumentsTable></DocumentsTable>
    </div>
</template>

<script>


    import DocumentsTable from './components/DocumentsTable'

    export default {
        name: 'app',
        components: {
            DocumentsTable
        }
    }
</script>

<style>
    body {
        position: absolute;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
    }

    body, html, #app, .container {
        height: 100%;
    }

    #app {
        height: 100%;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        width: 100%;
    }

    .has-text-centered {
        text-align: center !important;
    }

    .bodyoverflow {
        height: 80%;
    }

    .bodyoverflow tbody {
        overflow: scroll;
        height: 300px;
    }
</style>
