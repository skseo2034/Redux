# 리덕스 사용하기
1. store 생성.
```
function reducer(state, action) {
    if (state === undefined) {
        return {
            contents:[
                {id:1, title:'HTML', desc:'HTML is ..'},
                {id:2, title:'CSS', desc:'CSS is ..' }
            ]
        }
    }
    return newState
}
Redux.legacy_createStore(reducer);
```
2. state 사용해서 component 변경
```
function TOC() {
    const state = store.getState();
    let i = 0;
    let liTags = '';
    while( i < state.contents.length ) {
        liTags = liTags + `
            <li>
                <a href="${state.contents[i].id}">${state.contents[i].title}</a>
            </li>
        `;

        i = i + 1;
    }
    document.querySelector('#toc').innerHTML = `
        <nav>
            <ol>
                ${liTags}
            </ol>
        </nav>
    `
}
```
3. state 값 갱신을 통한 UI 갱신
    - 버튼등 click 를 통해 action 을 발생 시킨다.
    - 이 action 을 dispath 가 받아서 reducer 를 호출하고, 이 reducre 가 state 를 갱신한다.
    - state 가 바뀌면 subscribe 하고 있는 하고 있는 함수 들을 호출(render)하는 것을 통해 UI가 갱신된다.
```
 function TOC() {
    const state = store.getState();
       let i = 0;
       let aTitle, aDesc;
       while( i < state.contents.length ) {
           if (state.contents[i].id === state.selected_id) {
               aTitle = state.contents[i].title;
               aDesc = state.contents[i].desc;
           }
           i = i + 1;
       }
       document.querySelector('#content').innerHTML = `
           <article>
               <h2>${aTitle}</h2>
               ${aDesc}
           </article>
       `
   }
   function reducer(state, action) {

       if (state === undefined) {
           return {
               selected_id:1,
               contents:[
                   {id:1, title:'HTML', desc:'HTML is ..'},
                   {id:2, title:'CSS', desc:'CSS is ..' }
               ]
           }
       }
       let newSate;
       if (action.type === 'SELECT') {
           newSate = Object.assign({}, state, {selected_id:action.id});
       }
       console.log(action, state, newSate);

       return newSate;
   }
    const store = Redux.legacy_createStore(reducer); // store 가 reducer 를 호출 함.
    store.subscribe(article);  // state 가 변경될때 마다 article 함수가 호출 되어 ui를 갱신함.
    subject();
    TOC();
    control();
    article();
```
