<template>
	<div class="button button_start">
		<div class="button-title">{{ title }}</div>
		<div class="button-title-done">{{ titleDone }}</div>
		<div class="buttonImg">
			<div class="buttonCloud">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="black"
					width="18px"
					height="18px"
				>
					<path d="M0 0h24v24H0z" fill="none" />
					<path
						d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM19 18H6c-2.21 0-4-1.79-4-4s1.79-4 4-4h.71C7.37 7.69 9.48 6 12 6c3.04 0 5.5 2.46 5.5 5.5v.5H19c1.66 0 3 1.34 3 3s-1.34 3-3 3z"
					/>
				</svg>
			</div>
			<div class="buttonArrow">
				<div class="buttonArrow_up">
					<svg
						aria-hidden="true"
						focusable="false"
						role="img"
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 256 512"
					>
						<path
							fill="currentColor"
							d="M88 166.059V468c0 6.627 5.373 12 12 12h56c6.627 0 12-5.373 12-12V166.059h46.059c21.382 0 32.09-25.851 16.971-40.971l-86.059-86.059c-9.373-9.373-24.569-9.373-33.941 0l-86.059 86.059c-15.119 15.119-4.411 40.971 16.971 40.971H88z"
						></path>
					</svg>
				</div>
				<div class="buttonArrow_down">
					<svg
						aria-hidden="true"
						focusable="false"
						role="img"
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 256 512"
					>
						<path
							fill="currentColor"
							d="M88 166.059V468c0 6.627 5.373 12 12 12h56c6.627 0 12-5.373 12-12V166.059h46.059c21.382 0 32.09-25.851 16.971-40.971l-86.059-86.059c-9.373-9.373-24.569-9.373-33.941 0l-86.059 86.059c-15.119 15.119-4.411 40.971 16.971 40.971H88z"
						></path>
					</svg>
				</div>
			</div>
			<div class="buttonCheck">
				<svg viewBox="0 0 50 50">
					<path
						class="buttonCheck__path"
						d="M 5 20 L 20 40 L 45 10"
					></path>
				</svg>
			</div>
		</div>
		<div class="buttonBackground"></div>
	</div>
</template>

<script>
export default {
	name: "CloudSyncButton",
	props: {
		title: String,
		titleDone: String,
		syncProgress: Number,
		styling: Object,
		inSync: Boolean,
	},
	data() {
		return {
      syncProgressStartValue: 0,
      syncProgressStopValue: 100,
			btn: Object,
			loadBg: Object,
			// inSync: false,
		};
	},
	mounted() {
    this.loadBg = this.$el.querySelector('.buttonBackground');
    this.$el.style.background = this.styling.btnColor || "#725fdf"
    this.loadBg.style.background = this.styling.loadColor || "#81d427"
    this.$el.style.color = this.styling.strokeColor || "#fff"
    this.$el.querySelectorAll('svg').forEach(item => {
      item.style.fill = this.styling.strokeColor || "#fff"
    })
    this.$el.querySelector('.buttonCheck__path').style.stroke = this.styling.strokeColor || "#fff"    
	},
	watch: {
		syncProgress: {
			handler() {
				if (this.inSync) {          
          this.$el.classList.remove("button_start")
          if (this.syncProgress >= this.syncProgressStopValue) {
            this.$el.classList.remove("button_loading")
            this.$el.classList.add("button_loaded")
          } else {
            this.$el.classList.add("button_loading");          
          }
          this.loadBg.style.width = (this.syncProgress >= 100 ? 100 : this.syncProgress) + "%";
				}
			},
    },
    inSync: {
			handler() {
				if (this.inSync) {
					this.loadBg.style.width = this.syncProgressStartValue + "%";
					this.$el.classList.remove("button_start");
					this.$el.classList.add("button_loading");
				} else {
          this.$el.classList.remove("button_loaded");
          this.$el.classList.remove("button_loading");
          this.$el.classList.add("button_start");
          this.loadBg.style.width = this.syncProgressStartValue + "%";
				}
			},
		},
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .button {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    overflow: hidden;
    background: #725fdf;
    width: 150px;
    height: 50px;
    color: #fff;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    border-radius: 5px;
    font-family: arial;
    font-size: 20px;
    cursor: pointer;
    text-align: left;
  }
  .buttonCheck svg path {
    display: none;
  }
  .button_loading {
    background: #725fdf;
  }
  .button_loading .button-title {
    left: -100px;
  }
  .button_loading .buttonImg {
    right: 40px;
    z-index: 3;
  }
  .button_loading .buttonArrow_up {
    -webkit-animation: arrowTop 1.5s infinite 1s;
    animation: arrowTop 1.5s infinite 1s;
  }
  .button_loading .buttonArrow_down {
    -webkit-animation: arrowBottom 1.5s infinite 1s;
    animation: arrowBottom 1.5s infinite 1s;
  }
  .button_loaded .buttonImg {
    right: 40px;
    z-index: 4;
    -webkit-animation: cloudEnd 0.5s linear forwards 0.5s;
    animation: cloudEnd 0.5s linear forwards 0.5s;
  }
  .button_loaded .buttonArrow {
    display: none;
    -webkit-transition: 1s;
    -o-transition: 1s;
    transition: 1s;
  }
  .button_loaded .buttonCheck {
    position: absolute;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    width: 12px;
    height: 12px;
    -webkit-transition: 3s;
    -o-transition: 3s;
    transition: 3s;
    margin-top: 2px;
  }
  .button_loaded .buttonCheck svg {
    display: block;
    width: 100%;
    height: 100%;
  }
  .button_loaded .buttonCheck svg path {
    display: block;
    stroke: #fff;
    fill: none;
    stroke-width: 10px;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 65;
    stroke-dashoffset: 0;
    -webkit-transition: stroke-dasharray 0.4s;
    -o-transition: stroke-dasharray 0.4s;
    transition: stroke-dasharray 0.4s;
    -webkit-animation: dash 0.5s linear forwards;
    animation: dash 0.5s linear forwards;
  }
  .button_loaded .button-title-done {
    z-index: 6;
    -webkit-animation: textEnd 0.7s linear forwards 0.3s;
    animation: textEnd 0.7s linear forwards 0.3s;
  }
  .button_loaded .button-title {
    left: -100px;
  }
  .button-title {
    position: relative;
    left: 10px;
    width: 80px;
    -webkit-animation: textStart 1s;
    animation: textStart 1s;
    -webkit-transition: 1s;
    -o-transition: 1s;
    transition: 1s;
    margin-top: 2px;
  }
  .button-title-done {
    position: absolute;
    margin-left: 7px;
    width: 80px;
    left: -125px;
    margin-top: 2px;
  }
  .buttonImg {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    right: 10px;
    width: 36px;
    height: 36px;
    -webkit-animation: cloudStart 1s;
    animation: cloudStart 1s;
    -webkit-transition: 1s;
    -o-transition: 1s;
    transition: 1s;
  }
  .buttonCloud {
    width: 100%;
    height: 100%;
  }
  .buttonCloud svg {
    width: 36px;
    height: 36px;
  }
  .buttonArrow {
    position: absolute;
    overflow: hidden;
    width: 21px;
    height: 23px;
    border-radius: 50%;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    margin-top: 1px;
  }
  .buttonArrow_up,
  .buttonArrow_down {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    width: 30%;
    height: 100%;
  }
  .buttonArrow_up svg,
  .buttonArrow_down svg {
    width: 9px;
    height: 12px;
  }
  .buttonArrow_down svg {
    -webkit-transform: rotate(180deg);
    -ms-transform: rotate(180deg);
    transform: rotate(180deg);
  }
  .buttonBackground {
    position: absolute;
    z-index: 2;
    left: 0;
    width: 0;
    height: 55px;
    background: #81d427;
    -webkit-transition: width 0.4s;
    -o-transition: width 0.4s;
    transition: width 0.4s;
  }

  .panel {
    position: absolute;
    font-size: 12px;
    top: 0;
    width: 100%;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
  }
  .panel button {
    margin: 5px 10px;
  }

  @-webkit-keyframes textStart {
    0% {
      left: -100px;
    }
    85% {
      left: 12px;
    }
    100% {
      left: 10px;
    }
  }

  @keyframes textStart {
    0% {
      left: -100px;
    }
    85% {
      left: 12px;
    }
    100% {
      left: 10px;
    }
  }

  @-webkit-keyframes cloudStart {
    0% {
      right: -100px;
    }
    85% {
      right: 12px;
    }
    100% {
      right: 10px;
    }
  }

  @keyframes cloudStart {
    0% {
      right: -100px;
    }
    85% {
      right: 12px;
    }
    100% {
      right: 10px;
    }
  }

  @-webkit-keyframes arrowTop {
    0% {
      -webkit-transform: translateY(16px);
      transform: translateY(16px);
    }
    40%,
    60% {
      -webkit-transform: translateY(0);
      transform: translateY(0);
    }
    80%,
    100% {
      -webkit-transform: translateY(-16px);
      transform: translateY(-16px);
    }
  }

  @keyframes arrowTop {
    0% {
      -webkit-transform: translateY(16px);
      transform: translateY(16px);
    }
    40%,
    60% {
      -webkit-transform: translateY(0);
      transform: translateY(0);
    }
    80%,
    100% {
      -webkit-transform: translateY(-16px);
      transform: translateY(-16px);
    }
  }

  @-webkit-keyframes arrowBottom {
    0% {
      -webkit-transform: translateY(-16px);
      transform: translateY(-16px);
    }
    40%,
    60% {
      -webkit-transform: translateY(0);
      transform: translateY(0);
    }
    80%,
    100% {
      -webkit-transform: translateY(16px);
      transform: translateY(16px);
    }
  }

  @keyframes arrowBottom {
    0% {
      -webkit-transform: translateY(-16px);
      transform: translateY(-16px);
    }
    40%,
    60% {
      -webkit-transform: translateY(0);
      transform: translateY(0);
    }
    80%,
    100% {
      -webkit-transform: translateY(16px);
      transform: translateY(16px);
    }
  }

  @-webkit-keyframes dash {
    0% {
      stroke-dashoffset: 65;
    }
    100% {
      stroke-dashoffset: 0;
    }
  }

  @keyframes dash {
    0% {
      stroke-dashoffset: 65;
    }
    100% {
      stroke-dashoffset: 0;
    }
  }

  @-webkit-keyframes cloudEnd {
    0%,
    15% {
      width: 36px;
      height: 36px;
      right: 40px;
    }
    10% {
      width: 42px;
      height: 42px;
      right: 40px;
    }
    70% {
      right: 6px;
    }
    100% {
      right: 10px;
    }
  }

  @keyframes cloudEnd {
    0%,
    15% {
      width: 36px;
      height: 36px;
      right: 40px;
    }
    10% {
      width: 42px;
      height: 42px;
      right: 40px;
    }
    70% {
      right: 6px;
    }
    100% {
      right: 10px;
    }
  }

  @-webkit-keyframes textEnd {
    0% {
      left: -125px;
    }
    85% {
      left: 18px;
    }
    100% {
      left: 15px;
    }
  }

  @keyframes textEnd {
    0% {
      left: -125px;
    }
    85% {
      left: 18px;
    }
    100% {
      left: 15px;
    }
  }
</style>
