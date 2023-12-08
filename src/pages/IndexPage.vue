<template>
  <q-page class="flex column flex-center">

    <div class="q-pa-md">
      <div class="q-gutter-md row">

        <!-- Filtro de Localização -->
        <q-select
          filled
          v-model="locationFilter"
          use-input
          hide-selected
          fill-input
          input-debounce="0"
          :options="locationOptions"
          hint="Filtrar por Localização"
          style="width: 250px"
          dense
          clearable
        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey">
                Nenhuma localização disponível
              </q-item-section>
            </q-item>
          </template>
        </q-select>

        <!-- Filtro de Etapas -->
        <q-select
          filled
          v-model="stageFilter"
          use-input
          hide-selected
          fill-input
          input-debounce="0"
          :options="stageOptions"
          hint="Filtrar por Etapas"
          style="width: 250px; margin-left: 16px"
          dense
          clearable
        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey">
                Nenhuma etapa disponível
              </q-item-section>
            </q-item>
          </template>
        </q-select>

         <!-- Filtro de Voluntários -->
         <q-select
          filled
          v-model="volunteerFilter"
          use-input
          hide-selected
          fill-input
          input-debounce="0"
          :options="volunteerOptions"
          hint="Filtrar por Voluntário"
          style="width: 250px; margin-left: 16px"
          dense
          clearable
        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey">
                Nenhum voluntário disponível
              </q-item-section>
            </q-item>
          </template>
        </q-select>

        <!-- Filtro de Status -->
        <q-select
          filled
          v-model="statusFilter"
          use-input
          hide-selected
          fill-input
          input-debounce="0"
          :options="statusOptions"
          hint="Filtrar por Status"
          style="width: 250px margin-left: 16px"
          dense
          clearable
        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey">
                Nenhum status disponível
              </q-item-section>
            </q-item>
          </template>
        </q-select>

        <!-- Filtro de Data -->
        <q-input
          v-model="dateFilter"
          filled
          hint="Filtrar por Data"
          style="width: 250px; margin-left: 16px"
          dense
        />
      </div>
    </div>

    <div style="min-width: 1250px" class="q-pa-md">
      <q-table
        :rows="filteredRows"
        :columns="columns"
        row-key="name"
      />
    </div>
  </q-page>
</template>

<script setup>
import { ref, watchEffect } from 'vue';

const stringOptions = ['Google', 'Facebook', 'Twitter', 'Apple', 'Oracle'];

const rows = [
  {
    project: 'Doce Lar',
    address: 'Rua Padre Lima e Sá',
    stage: 'Finalizado',
    volunteer: 'Ricardo',
    status: 'Concluído',
    date: '01/01/2023'
  },
  {
    project: 'Mais Escolas',
    address: 'Rua Mamanguape',
    stage: 'Inicial',
    volunteer: 'João',
    status: 'Em andamento',
    date: '02/01/2023'
  },
  {
    project: 'Saúde para todos',
    address: 'Avenida Antônio Falcão',
    stage: 'Inicial',
    volunteer: 'Iago',
    status: 'Em andamento',
    date: '03/01/2023'
  },
  {
    project: 'Tecnologia',
    address: 'Agamenon Magalhães',
    stage: 'Ajustes',
    volunteer: 'Iago',
    status: 'Em andamento',
    date: '03/01/2023'
  },
  {
    project: 'Acesso a educação',
    address: 'Avenida Domingos Ferreira',
    stage: 'Final',
    volunteer: 'Eduardo',
    status: 'Em andamento',
    date: '03/01/2023'
  },
  {
    project: 'Recife Limpo',
    address: 'Rua Padre Carapuceiro',
    stage: 'Inicial',
    volunteer: 'Iago',
    status: 'Em andamento',
    date: '03/01/2023'
  },
  {
    project: 'Acessibilidade para todos',
    address: 'Avenida Antônio Falcão',
    stage: 'Em andamento',
    volunteer: 'Iago',
    status: 'Em andamento',
    date: '03/01/2023'
  },
];

const locationOptions = Array.from(new Set(rows.map(row => row.address)));
const stageOptions = Array.from(new Set(rows.map(row => row.stage)));
const statusOptions = Array.from(new Set(rows.map(row => row.status)));
const volunteerOptions = Array.from(new Set(rows.map(row => row.volunteer)));

const locationFilter = ref(null);
const stageFilter = ref(null);
const statusFilter = ref(null);
const dateFilter = ref(null);
const volunteerFilter = ref(null);


const columns = [
  {
    name: 'project',
    required: true,
    label: 'Projeto',
    align: 'left',
    field: row => row.project,
    format: val => `${val}`,
    sortable: true
  },
  {
    name: 'address',
    label: 'Endereço',
    align: 'left',
    field: row => row.address,
    sortable: true
  },
  {
    name: 'stage',
    label: 'Etapa',
    align: 'left',
    field: row => row.stage,
    sortable: true
  },
  {
    name: 'volunteer',
    label: 'Voluntário',
    align: 'left',
    field: row => row.volunteer,
    sortable: true
  },
  {
    name: 'status',
    label: 'Status',
    align: 'left',
    field: row => row.status,
    sortable: true
  },
  {
    name: 'date',
    label: 'Data',
    align: 'left',
    field: row => row.date,
    sortable: true
  }
];

const filteredRows = ref(rows);

watchEffect(() => {
  filteredRows.value = rows.filter(row => {
    const locationMatch = !locationFilter.value || row.address.includes(locationFilter.value);
    const stageMatch = !stageFilter.value || row.stage.includes(stageFilter.value);
    const statusMatch = !statusFilter.value || row.status.includes(statusFilter.value);
    const dateMatch = !dateFilter.value || row.date.includes(dateFilter.value);
    const volunteerMatch = !volunteerFilter.value || row.volunteer.includes(volunteerFilter.value);

    return locationMatch && stageMatch && statusMatch && dateMatch && volunteerMatch;
  });
});
</script>
