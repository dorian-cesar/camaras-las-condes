<template>
    <table>
        <thead>
            <tr>
            <th>Patente</th>
            <th>Estado</th>
            </tr>
        </thead>
        <!-- Por cada objeto creamos una fila -->
        <tbody v-for="(item,index) in urldata" :key="index">
            <tr>
                <td>{{item.patente}}</td>
                <td>{{ item.estado==="active" ? 'Activo' : 'Inactivo' }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    props: {
        urldata: Object
    },
}
</script>

<style scoped>
.itemlogo table {
  width: 100%;
  background-color: #360461;
  color: white;
  border: 1px solid black;
  border-collapse: collapse;
}

.itemlogo table th, td {
  border-bottom: 1px solid black;
}

.itemlogo td {
  background-color: #524161;
}
</style>