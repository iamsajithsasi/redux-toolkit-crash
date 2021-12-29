# Redux Toolkit Crash

### + store.js

```
import { configureStore } from "@reduxjs/toolkit";

// reducers
import AuthenticationReducer from "./authentication";

export const store = configureStore({
  reducer: {
    authentication: AuthenticationReducer,
  },
});

```

### + authentication.js

```
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: {
    isLogged: false,
    loginData: null,
  },
};

export const authenticateUser = createAsyncThunk(
  "authentication/user",
  async (data, thunkAPI) => {
    const response = await axios.post(loginUrl, data);
    return response;
  }
);

export const authenticationSlice = createSlice({
  name: "authentication",
  initialState,
  reducers: {
    updateUser: (state, action) => {
      state.value = {
        isLogged: action.payload.isLogged,
        loginData: action.payload,
      };
    },
  },
  extraReducers: {
    [authenticateUser.fulfilled]: (state, action) => {
      state.isLogged = true;
      state.loginData = action.payload;
    },
    [authenticateUser.rejected]: (state, action) => {
      state.isLogged = false;
      state.loginData = null;
    },
    [authenticateUser.pending]: (state, action) => {
      state.isLogged = false;
      state.loginData = null;
    },
  },
});

export const { updateUser } = authenticationSlice.actions;

export default authenticationSlice.reducer;

```

### + somecomponent.js

```
import { authenticateUser } from "../library/store/authentication";
import { useDispatch, useSelector } from "react-redux";

export default function SamplePage() {
    const dispatch = useDispatch();
    const getStateFromReduxStore = useSelector((state) => state.authentication.value);
    submitData(data) {
        dispatch(authenticateUser(data));
    }
}

```
