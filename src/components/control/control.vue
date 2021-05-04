<template src="@/components/control/control.html"></template>

<script>
export default {
  name: 'selectToday',
  data(){
    return {
      today:{
        Year: 0,
        Month: 0,
        date: 0,
        day: 0
      },
      calender:{
        Year: 0,
        Month: 0,
        date: 0,
        day: 0
      },
      value : [{
                id:"2021-3-27",
                len : 2,
                content:["abc","456"]
                },
                {
                id:"2021-4-19",
                len : 1,
                content:["hhh"]
                }],
      todo : [],
      isAdd: false,
      isEdit: false,
      nowId: "",
      nowValue: ""
    }
  },
  mounted(){
    this.getToday()
  },
  methods:{
    addValue(){
      this.isAdd = true
    },
    submitHandler(){
      const a = this.value.filter(e => e.id == this.nowId)
      if(a.length > 0){
        a[0].content.push(this.nowValue)
        a[0].len = a[0].content.length
      }else{
        this.todo[0].id = this.nowId
        this.todo[0].content = [this.nowValue]
        this.todo[0].len = this.todo[0].content.length
        this.value.push(this.todo[0])
      }
      this.nowValue = ""
      this.isAdd = false
    },
    deleteValue(index){
      const valuesIndex = this.value.findIndex(e => e.id == this.nowId)
      this.value[valuesIndex].content.splice(index)

      if(this.value[valuesIndex].content.length > 0){
        this.value[valuesIndex].len = this.value[valuesIndex].content.length
      }else{
        this.value.splice(valuesIndex)
      }
    },
    editValue(index){
      const a = this.value.filter(e => e.id == this.nowId)
      a[0].content[index] = this.nowValue
      a[0].len = a[0].content.length
      this.nowValue = ""
      this.isEdit = false
    },
    isEditTure(){
      this.isEdit = true
    },
    getContent({Year, Month, date}){
      return this.value.filter(e => e.id == `${Year}-${Month}-${date}`)
    },
    showValue(obj){
      this.nowId = obj
      this.todo.splice(0,this.todo.length)
      const a = this.value.filter(e => e.id == obj)
      this.todo.push(a.length > 0 ? a[0] : {})
      this.isEdit = false
      this.isAdd = false
    },
    setItemId({Year, Month, date}){
      return `${Year}-${Month}-${date}`
    },
    addStyle({Year, Month, date}){
      if(this.today.Year == Year && this.today.Month == Month && this.today.date == date){
        return "today"
      }else if(this.today.Year == Year && this.today.Month == Month){
        return "toMonth"
      }else{
        return "notMonth"
      }
    },
    getToday(){
      const dateNow = new Date()
      this.today.Year = dateNow.getFullYear()
      this.today.Month = dateNow.getMonth()
      this.today.date = dateNow.getDate()
      this.today.day = dateNow.getDay()
      this.calender.Year = dateNow.getFullYear()
      this.calender.Month = dateNow.getMonth()
      this.calender.date = dateNow.getDate()
      this.calender.day = dateNow.getDay()
    },
    adjectYear(fix){
      this.calender.Year += fix
    },
    adjectMonth(fix){
      this.calender.Month += fix
      if(this.calender.Month < 1){
        this.calender.Year -= 1
        this.calender.Month = 12
      }else if(this.calender.Month > 12){
        this.calender.Year += 1
        this.calender.Month = 1
      }
    }
  },
  computed: {
    firstCalenderDay(){
      const mDate = new Date(this.calender.Year,this.calender.Month,1)
      const date = new Date(this.calender.Year,this.calender.Month,1 - mDate.getDay())
      return{
        Year: date.getFullYear(),
        Month: date.getMonth(),
        date: date.getDate(),
        day: date.getDay()
      }
    },
    monthDay(){
      const data = []
      let date
      for(let i=0; i<42; i++){
        date = new Date(this.firstCalenderDay.Year,this.firstCalenderDay.Month,this.firstCalenderDay.date + i)
        const d = {
          Year: date.getFullYear(),
          Month: date.getMonth(),
          date: date.getDate(),
          day: date.getDay()
        }
        d.content = this.getContent(d)
        data.push(d)
      }
      return data
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calender{
  font-size: 20px;
}
.weekDay, .week{
  display: flex;
  justify-content: space-around;
  border-bottom: 1px solid #ddd;
}
.weekDay > div{
  line-height: 30px;
}
.week{
  border-right: 1px solid #ddd;
}
.day{
  position: relative;
  flex: auto;
  height: 100px;
  line-height: 30px;
  border-left: 1px solid #ddd;
}
.day::after{
  position: absolute;
  content: attr(date-day);
  top: 0;
  right: 0;
}
.today{
  background-color: #FFFFF0;
}
.toMonth{
  background-color:#F0FFF0;
}
.notMonth::after{
  color: #D3D3D3
}
.edit{
  position: absolute;
  left:0px;
  width:100%;
  height:100%;
}
.edit > button{
  width:100%;
  height:100%;
}
.tag{
  position: absolute;
  left: 0px;
  width:100%;
  height:100%;
  z-index: 2;
}
.tag > button{
  position: relative;
  top:50%;
  transform:translateY(-50%);
}

</style>
