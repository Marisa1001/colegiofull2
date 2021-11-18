<template>
<q-page>
  <div class="row">
    <div class="col-12">
      <q-form @submit.prevent="crear">
        <div class="row q-pa-xs">
          <div class="col-2">
            <q-input outlined dense label="nombre" required v-model="estudiante.paterno"/>
          </div>
          <div class="col-2">
            <q-input outlined dense label="materno" required v-model="estudiante.materno"/>
          </div>
          <div class="col-2">
            <q-input outlined dense label="nombres" required v-model="estudiante.nombres"/>
          </div>
          <div class="col-1">
            <q-input outlined dense label="ci" required v-model="estudiante.ci"/>
          </div>
          <div class="col-1">
            <q-input outlined dense label="sexo" required v-model="estudiante.sexo"/>
          </div>
          <div class="col-2">
            <q-input outlined dense label="fechanacimiento" required v-model="estudiante.fechanacimiento"/>
          </div>
          <div class="col-2">
            <q-input outlined dense label="celular" required v-model="estudiante.celular"/>
          </div>
          <div class="col-4 flex flex-center">
            <q-btn type="submit" outlined dense icon="send" :color="boolcrear?'positive':'yellow'" :label="boolcrear?'Insertar':'Modificar'"/>
          </div>
        </div>
      </q-form>
    </div>
    <div class="col-12">
      <q-table title="Estudiantes" :columns="columns" :rows="estudiantes" dense>
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="id" :props="props">
              <q-badge>{{props.row.id}}</q-badge>
            </q-td>
            <q-td key="paterno" :props="props">
              <q-badge color="teal" >{{props.row.paterno}}</q-badge>
            </q-td>
            <q-td key="materno" :props="props">
              <q-badge color="negative">{{props.row.materno}}</q-badge>
            </q-td>
            <q-td key="nombres" :props="props">
              <q-badge color="negative">{{props.row.nombres}}</q-badge>
            </q-td>
            <q-td key="ci" :props="props">
              <q-badge color="negative">{{props.row.ci}}</q-badge>
            </q-td>
            <q-td key="sexo" :props="props">
              <q-badge color="negative">{{props.row.sexo}}</q-badge>
            </q-td>
            <q-td key="fechanacimiento" :props="props">
              <q-badge color="negative">{{props.row.fechanacimiento}}</q-badge>
            </q-td>
            <q-td key="celular" :props="props">
              <q-badge color="negative">{{props.row.celular}}</q-badge>
            </q-td>
            <q-td key="opciones" :props="props">
              <q-btn @click="eliminar(props.row)" size="xs" icon="delete" color="negative"  label="Eliminar"/>
              <q-btn @click="modificar(props.row)" size="xs" icon="edit" color="primary"  label="Modifica"/>
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </div>
  </div>
</q-page>
</template>
<script>
export default {
  data(){
    return {
      estudiantes:[],
      estudiante:{paterno:''},
      boolcrear:true,
      columns:[
        {name:'id',label:'#',field:'id'},
        {name:'paterno',label:'Paterno',field:'paterno'},
        {name:'materno',label:'Materno',field:'materno'},
        {name:'nombres',label:'Nombres',field:'nombres'},
        {name:'ci',label:'Ci',field:'ci'},
        {name:'sexo',label:'Sexo',field:'sexo'},
        {name:'fechanacimiento',label:'Fechanacimiento',field:'fechanacimiento'},
        {name:'celular',label:'Celular',field:'celular'},
        {name:'opciones',label:'Opciones',field:'opciones'},
      ]
    }
  },
  created() {
    this.misdatos()
  },
  methods:{
    crear(){
      if (this.boolcrear)
      this.$api.post('/estudiante',this.estudiante).then(res=>{
        this.estudiante={}
        this.misdatos()
      })
      else
      this.$api.put('/estudiante/'+this.estudiante.id,this.estudiante).then(res=>{
        this.estudiante={}
        this.misdatos()
        this.boolcrear=true
      })
    },
    misdatos(){
      this.$api.get('/estudiante').then(res=>{
        this.estudiantes=res.data
      })
    },
    eliminar(estudiante){
      if (confirm("Seguro de eliminar?"))
      this.$api.delete('/estudiante/'+estudiante.id).then(res=>{
        this.misdatos()
      })
    },
    modificar(estudiante){
      this.estudiante=estudiante
      this.boolcrear=false
    }
  }
}
</script>

<style scoped>

</style>
