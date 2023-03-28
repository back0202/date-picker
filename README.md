# Date Picker
npm install 한후 package.json에 입력된 scripts를 참고하여 dev server를 실행하거나 build 하면 됩니다.

# 로직
1. 년,월,일 은 js로 데이터를 넣어준다.
2. 년,월,일 박스를 클릭시 달려이 나온다.
3. 달력의 월과 일을 구한다.
- 일을 구할때
```
const numberOfDates = new Date(
      this.#calendarDate.year,
      this.#calendarDate.month + 1,
      0,
    ).getDate();
```
- 총 일수를 구한다.
- 첫번째 일의 속성에 gridColumnStart을 넣어준다.
```
 fragment.firstChild.style.gridColumnStart =
      new Date(this.#calendarDate.year, this.#calendarDate.month, 1).getDay() +
      1;
```
- 토요일과 일요일 색을 넣어준다.
- 토요일
```
  colorSaturday() {
    const saturdayEls = this.calendarDatesEl.querySelectorAll(
      `.date:nth-child(7n+${
        7 -
        new Date(this.#calendarDate.year, this.#calendarDate.month, 1).getDay()
      })`,
    );
    for (let i = 0; i < saturdayEls.length; i++) {
      saturdayEls[i].style.color = 'blue';
    }
  }
```
- 첫번째 토요일 구하는 방법은 없을까 7-첫번째요일
- 일요일 구하는 방법은 8-(첫번째 요일%7)

4. 이전버튼과 다음 버튼클릭시 위에서 했던 달, 일을 다시 구해준다.
5. 오늘 날짜 마킹
```
  markToday() {
    const currentDate = new Date();
    const currentMonth = currentDate.getMonth();
    const currentYear = currentDate.getFullYear();
    const today = currentDate.getDate();
    if (
      currentYear === this.#calendarDate.year &&
      currentMonth === this.#calendarDate.month
    ) {
      this.calendarDatesEl
        .querySelector(`[data-date='${today}']`)
        .classList.add('today');
    }
  }
```
- 오늘 날짜를 구하고 현재 달력의 표시와 비교해서 같다면 마킹

6. 선택 날짜 마킹
