<template>
	<div class="col" :class="colClass" :style="colStyle">
		<slot></slot>
	</div>
</template>

<script>
    let validator = (value) => {
        let keys = Object.keys(value);
        let valid = true;
        keys.forEach(key => {
            if (!['span', 'offset'].includes(key)) {
                valid = false
            }
        });
        return valid
    };
    export default {
        name: "v-col",
        data() {
            return {
                gutter: 0,
            }
        },
        props: {
            span: {
                type: [String, Number],
            },
            offset: {
                type: [String, Number]
            },
            ipad: {type: Object, validator,},
            narrowPc: {type: Object, validator,},
            pc: {type: Object, validator,},
            widePc: {type: Object, validator,}
        },
        computed: {
            colStyle() {
                return {paddingLeft: this.gutter / 2 + 'px', paddingRight: this.gutter / 2 + 'px'}
            },
            colClass() {
                let {span, offset, ipad, pc, widePc} = this;
                let createClass = this.createClass;
                return [
                    ...createClass({span, offset}),
                    ...createClass(ipad, 'ipad-'),
                    ...createClass(pc, 'pc-'),
                    ...createClass(widePc, 'wide-pc')
                ]
            }
        },
        methods: {
            createClass(obj, str = '') {
                if (!obj) return [];
                let arr = [];
                if (obj.span) {
                    arr.push(`col-${str}${obj.span}`)
                }
                if (obj.offset) {
                    arr.push(`offset-${str}${obj.offset}`)
                }
                return arr
            }
        }
    }
</script>

<style lang="scss" scoped>
	.col {
		min-height: 50px;
		flex: auto;
		&:nth-child(odd) > div {
			background: #00A0E9;
		}

		&:nth-child(even) > div {
			background: #59BCEF;
		}

		$class-prefix: col-;
		@for $n from 1 through 24 {
			&.#{$class-prefix}#{$n} {
				width: ($n / 24) *100%;
			}
		}
		$class-prefix: offset-;
		@for $n from 1 through 24 {
			&.#{$class-prefix}#{$n} {
				margin-left: ($n / 24) * 100%;
			}
		}
		@media (min-width: 577px) {
			$class-prefix: col-ipad-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					width: ($n / 24) * 100%;
				}
			}
			$class-prefix: offset-ipad-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					margin-left: ($n / 24) * 100%;
				}
			}
		}
		@media (min-width: 993px) {
			$class-prefix: col-pc-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					width: ($n / 24) * 100%;
				}
			}
			$class-prefix: offset-pc-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					margin-left: ($n / 24) * 100%;
				}
			}
		}
		@media (min-width: 1201px) {
			$class-prefix: col-wide-pc-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					width: ($n / 24) * 100%;
				}
			}
			$class-prefix: offset-wide-pc-;
			@for $n from 1 through 24 {
				&.#{$class-prefix}#{$n} {
					margin-left: ($n / 24) * 100%;
				}
			}
		}

		div {
			background: #2b85e4;
			height: 100%;
		}
	}
</style>