<template>
  <div class="table-responsive" style="">
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th style="width: 75px;">{{$t('file.id')}}</th>
          <th>{{$t('file.originalName')}}</th>
          <th>{{$t('file.size')}}</th>
          <th>{{$t('file.mimeType')}}</th>
          <th>{{$t('file.createdAt')}}</th>
          <th />
        </tr>
      </thead>
      <tbody>
        <tr v-for="file in files" :key="file.id">
          <td>{{ file.id }}</td>
          <td>{{ file['originalName'] }}</td>
          <td>{{ bytes(file['size']) }}</td>
          <td>{{ file['mimeType'] }}</td>
          <td>{{ moment(file['createdAt']).format('DD-MM-YYYY HH:mm') }}</td>
          <td>
            <a href="#" @click.stop="download(file)">
              <span class="fa fa-download" aria-hidden="true" />
              <span class="sr-only">{{$t('file.download')}}</span>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import filesize from '../../../node_modules/filesize'
  import axios from '../../interceptor'

  export default {
    name: 'FilesTable',
    props: {
      files: {
        type: Array,
        default: function() {
          return []
        }
      }
    },
    methods: {
      bytes (size) {
        return filesize(size)
      },
      download (file) {
        axios({
          url: process.env.API_URL + '/files/download/' + file.id,
          method: 'GET',
          responseType: 'blob', // important
        }).then((response) => {
          const url = window.URL.createObjectURL(new Blob([response.data]));
          const link = document.createElement('a');
          link.href = url;
          link.setAttribute('download', file.originalName);
          document.body.appendChild(link);
          link.click();
        });
      }
    }
  }
</script>
