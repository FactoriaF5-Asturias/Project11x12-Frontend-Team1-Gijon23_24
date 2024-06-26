<script setup>
import { defineProps, watch } from 'vue';
import { useRouter } from 'vue-router';
import { ref } from 'vue';

const router = useRouter();

const redirectInscriptions = (campamentoName) => {
    router.push(`/inscription/${campamentoName}`);
  };

const props = defineProps({
  campamento: Object,
});

let colorFig = '';
let navEventColor = '';
let btnColor = '';

const setColors = () => {
  switch (props.campamento.name) {
    case 'Navidad':
      colorFig = '#6E8EBC';
      navEventColor = '#6E8EBC';
      btnColor = '#3D6BAD';
      break;
    case 'Semana Santa':
      colorFig = '#5FC68E';
      navEventColor = '#5FC68E';
      btnColor = '#497C48';
      break;
    case 'Verano':
      colorFig = '#F5D872';
      navEventColor = '#F5D872';
      btnColor = '#F6FF90';
      break;
    default:
      colorFig = 'transparent'; 
      navEventColor = 'transparent'; 
      btnColor = 'transparent'; 
  }
};

const isFlipped = ref(false);

const flipCard = (event) => {
  const card = event.currentTarget.closest('.card');
  const backFace = card.querySelector('.card__face--back');
  isFlipped.value = !isFlipped.value;
  if (isFlipped.value) {
    backFace.style.backgroundColor = colorFig;
  }
};

watch(() => props.campamento, setColors, { immediate: true });


</script>

<template>
  <div id="card-container">
  <div class="card" @click="flipCard">
    <div :class="{ 'card__inner': true, 'is-flipped': isFlipped }">
      <div class="card__face card__face--front">
        <div class="color_fig" :style="{ backgroundColor: colorFig }"></div>
    <img class="mainImg" :src="'/img/' + campamento.image" alt="Imagen de {{ campamento.name }}" />

    <div class="dates_event">
      <h5 :class="campamento.name === 'Navidad' ? 'navidad' : campamento.name === 'Verano' ? 'verano' : ''">{{
      campamento.name }}</h5>
      <div class="text_event">
        <h6><strong>Fecha inicio/fin:</strong>
          <p>{{ campamento['start date'] }} - {{ campamento['end date'] }} </p>
        </h6>
      </div>
    </div>

    <p class="descrip_events">{{ campamento['description'] }}</p>

    <div class="main_event">
      <div :style="'background-color: ' + navEventColor" class="nav_event"
        :class="{ 'black_text_verano': props.campamento.name === 'Verano' }">
        <p><img class="flag" src="../../assets/icons/flag.svg" alt=""> {{ campamento['date flag'] }} </p>
        <p><img class="flag" src="../../assets/icons/people.svg" alt=""> {{ campamento['date user'] }} </p>
        <p class="diferent">{{ campamento['date diner'] }}</p>
        <button :style="'background-color: ' + btnColor" type="button" id="btn_ins" @click="redirectInscriptions(campamento.name)">INSCRIBIRME</button>
      </div>
    </div>
    </div>
    <div class="card__face card__face--back">
        <p>Más información</p>
        <p class="descrip_events">{{ campamento['description'] }}</p>
      </div>
    </div>
  </div>
</div>
</template>

<style scoped lang="scss">

#card-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  display: flex;
  width: 100%;
  max-width: 500px;
  height: 355px;
  border: 1.3px solid rgb(146, 141, 141);
  border-radius: 0 10px 10px 0;
  overflow: hidden;

  .color_fig {
  width: 5vw;
  height: 75%;
  position: absolute;
  border-radius: 0 0 10px 0;
  top: 0; 
  left: 0; 
}

  .mainImg {
    width: 95%;
    height: 60%;
    z-index: 1;
    float: right;
    margin-left: auto;
    object-fit: cover;
  }

  .dates_event {
    display: flex;
    flex-direction: row;
    padding: 3px 0 0 80px;
    z-index: 1;

    h5 {
      font-size: large;
      font-weight: 700;
    }

    .text_event {
      width: 40%;
      display: flex;
      flex-direction: column;
      text-align: center;
      margin-left: 15px;
      flex-grow: 1;

      h6 {
        font-size: xx-small;
      }

      p {
        font-size: xx-small;
        font-weight: 700;
        margin-bottom: 0;
      }
    }
  }

  .descrip_events {
    font-size: small;
    text-align: center;
    padding-top: 7px;
    font-weight: 500;
    margin: 10px;
  }

  .main_event {
    display: flex;
    justify-content: space-between;
    width: 95%;

    .nav_event {
      width: 100%;
      display: flex;
      justify-content: space-around;
      gap: 14px;
      align-items: center;
      padding-right: 10px;
      font-weight: 600;

      .flag {
        width: 35px;
        image-rendering: auto;
        padding: 5px;
      }

      p {
        font-size: xx-small;
        color: white!important;;
        margin-bottom: 0;
      }

      .diferent {
        font-weight: 350;
      }

      #btn_ins {
        font-size: xx-small;
        font-weight: 500;
        border-radius: 20px;
        border: none;
        color: white;
        padding: 6px;
      }
    }
  }
}

.nav_event.black_text_verano p,
.nav_event.black_text_verano #btn_ins {
  color: black !important;
  font-weight: 650;
}

.navidad {
  padding-right: 45px;
}

.verano {
  padding-right: 45px;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  box-shadow: 0px 3px 18px 3px rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
}

.card__face--front {
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;


}

.card__face--back {
  background-color: inherit; 
  transform: rotateY(180deg);
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

@media only screen and (max-width: 1258px) and (min-width: 649px) {
  .card {
    .main_event {
      padding: 0 4px;

      .nav_event {
        padding-top: 5px;

        .flag {
          width: 24px;
          image-rendering: auto;
        }
      }
    }
  }
}

@media only screen and (max-width: 1217px) and (min-width: 596px) {
  .card {
    .main_event {
      padding: 0 4px;


      .nav_event {
        padding-top: 5px;

        .flag {
          width: 30px;
          image-rendering: auto;
        }
      }
    }
  }
}

@media only screen and (max-width: 1214px) and (min-width: 649px) {
  .card {
    .main_event {
      padding: 0 4px;

      .nav_event {
        padding-top: 5px;
      }
    }
  }
}

@media only screen and (max-width: 1204px) and (min-width: 649px) {
  .card {
    .color_fig {
      width: 5vw;
    }

    .dates_event {
      padding-left: 75px;

      h5 {
        font-size: small;
      }

      .text_event {
        margin-left: 29px;
      }
    }

    .descrip_events {
      padding-top: 10px;
      margin-left: 2px;
    }

    .main_event {
      padding: 0 4px;
    }
  }
}

@media only screen and (max-width: 1267px) and (min-width: 649px) {
  .card {
    .color_fig {
      width: 5vw;
    }

    .dates_event {
      padding-left: 75px;

      .text_event {
        margin-left: 30px;
      }
    }

    .descrip_events {
      padding-top: 12px;
      margin-right: 1px;
    }

    .main_event {
      padding: 0 4px;

      .nav_event {
        .flag {
          width: 32px;
        }
      }
    }
  }
}


 
@media only screen and (min-width: 414px) and (max-width: 896px) {
  .card {
    .descrip_events {
      padding: 14px 40px .1px 48px;
    }
  }
}

@media only screen and (min-width: 412px) and (max-width: 915px) {
  .card {
    .descrip_events {
      padding: 14px 40px .1px 48px;
    }
  }
}
@media only screen and (min-width: 375px) and (max-width: 667px) {
  .card {
    .color_fig {
      width: 17vw;
    }

    .dates_event {
      justify-content: space-around;
      margin-left: 10px;

      h5 {
        margin-left: 38px;
      }

      .text_event {
        margin-left: -1px;
      }
    }

    .main_event {
      padding: 0 7px;

      .nav_event {
        .flag {
          width: 5vw;
        }
      }
    }
  }
}

@media only screen and (min-width: 360px) and (max-width: 740px) {
  .card {
    .color_fig {
      width: 10vw;
    }
    .descrip_events {
      padding: 2px 2px .9px 5px;
    }
    .dates_event {
      justify-content: space-around;
      margin-right: -20px;

      h5 {
        margin-left: -9px;
      }

      .text_event {
        margin-left: -50px;
      }
    }

    .main_event {
      padding: 0 6px;

      .nav_event {
        justify-content:space-between;
        padding: 4px ;
        .flag {
          width: 10px;
        }
      }
    }
  }
}

@media only screen and (max-width: 768px) and (min-width: 506px) {
  .card {
    .main_event {
      padding: 0 8px;

      .nav_event {

        .flag {
          width: 3vw;
        }
      }
    }
  }
}

@media only screen and (max-width: 1024px) and (min-width: 675px) {
  .card {
    .color_fig {
      width: 5vw;
    }

    .dates_event {
      justify-content: space-around;
      margin-left: 7px;

      h5 {
        margin-left: 10px;
      }

      .text_event {
        margin-left: -20px;
      }
    }
.descrip_events{
  padding-top: 3px;
}
    .main_event {
      padding: 0 7px;

      .nav_event {
        padding-top: 1px;
        .flag {
          width: 3.8vw;
        }
      }
    }
  }
}

@media only screen and (min-width: 1175px) and (max-width: 779px) {
  .card {
    .color_fig {
      width: 5vw;
    }

    .dates_event {
      justify-content: space-between;
      margin-left: 7px;

      h5 {
        margin-left: 10px;
      }

      .text_event {
        margin-left: -10px;
      }
    }

    .main_event {
      padding: 0 7px;

      .nav_event {
        .flag {
          width: 2.5vw;
        }
      }
    }
  }
} 
</style>
