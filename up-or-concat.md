
>数据更新或者合并

#up or concat
*转载需加来源*

**update action**
```
import * as types from '../constants/ActionTypes'

export default function upDate (upDate) {
    return {
        type: types.RECEIVE_UPDATE,
        upDate
   
```
**需要调用的action**
```
import reqwest from 'reqwest'
import * as types from '../constants/ActionTypes'
import handleResp from 'utils/handleResp'  //自己封装的方法（只是处理请求数据）

export default function fetchCinema (param) {
    return (dispatch) => {
        const url = `${API}/***` //请求链接
        return reqwest({url, 'data': param})
            .then(resp => {
                handleResp(resp, () => {
                    dispatch(receiveCinema(resp.retval))
                })
                return resp
            })
    }
}

function receiveCinema (cinema) {
    return {
        type: types.RECEIVE_CINEMA,
        cinema
    }
}

```
**reducers**
```
import * as types from '../constants/ActionTypes'

const initialState = {
    cinema: {
        attach: {}
    },
    movieList: [],
    upDate: false
}

export default function cinema (state = initialState, action) {
    switch (action.type) {
    case types.RECEIVE_CINEMA:
        {
            return Object.assign({}, state, {
                cinema: action.cinema.cinema,
                movieList: state.upDate ? action.cinema.movieList : state.movieList.concat(action.cinema.movieList)
            })
        }
    case types.RECEIVE_UPDATE:
        {
            let objectMap = Object.assign({}, state, {
                upDate: action.upDate
            })

            return objectMap
        }

    default:
        return state
    }
}

```
**调用**
只需要在接口前调用actions.upDate(true)或者actions.upDate(false)
