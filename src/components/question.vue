<template>
    <div class="question" :id="`question-${questionId}`">
        <label class="question__title">
            <input class="question__title-check" type="checkbox" name="check" v-model="status" @change="changeItem()">
            <label class="question__title-label" for="check"></label>
            <p class="question__title-text">{{ info.title }}</p>
        </label>
        <div class="question__answers" v-if="status">
            <label
                class="question__answers-item"
                v-for="(answer, index) in info.answers"
                :key="'answer-label-' + index"
                :class="{'active': answers[index]}"
            >
                <input
                    class="question__answers-item-check"
                    type="checkbox"
                    :name="'answer-' + index"
                    v-model="answers[index]"
                    @change="changeItem()"
                >
                {{ answer }}
            </label>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "questionItem",

    data () {
        return {
            status: false,

            answers: {}
        }
    },

    props: {
        info: Object,
        questionId: Number,
    },

    methods: {
        changeItem() {
            let countItem = 0
            
            for (const key in this.answers) {
                this.answers[key] ? countItem++ : false;
            }

            this.$emit('changeQuestion', {
                id: this.questionId,
                status: countItem || !this.status ? true : false,
            })

        }
    }
};
</script>

<style lang="scss">
    .question {
        background-color: rgb(224, 224, 224);
        padding: 8px 16px;
        border-radius: 8px;

        &__title {
            display: flex;
            gap: 8px;
            width: 100%;

            cursor: pointer;

            &-check {
                position: absolute;
                opacity: 0;

                &+label {
                    display: inline-flex;
                    align-items: center;
                    user-select: none;

                    &::before {
                        content: '';
                        display: inline-block;
                        width: 12px;
                        height: 12px;
                        flex-shrink: 0;
                        flex-grow: 0;
                        border: 1px solid rgb(49, 49, 49);
                        margin-right: 0.5em;
                        background-repeat: no-repeat;
                        background-position: center center;
                        background-size: 50% 50%;
                    }

                }

                &:checked+label::before {
                    border-color: #ef0b29;
                    background-color: #ef0b29;
                    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26 2.974 7.25 8 2.193z'/%3e%3c/svg%3e");
                }

            }

            &-text {
                width: 100%;
                text-align: left;
            }

        }

        &__answers {
            display: flex;
            gap: 4px;
            flex-wrap: wrap;

            margin-top: 8px;
            padding-top: 8px;

            border-top: 1px solid rgb(119, 119, 119);

            &-item {
                padding: 4px 8px;
                border: 1px solid rgb(49, 49, 49);
                border-radius: 50px;
                white-space: nowrap;

                transition: .4s ease;
                cursor: pointer;

                &-check{
                    position: absolute;
                    opacity: 0;
                }

            }

            .active{
                background-color: rgb(49, 49, 49);
                color: rgb(255, 255, 255);
            }

        }

    }
</style>
  