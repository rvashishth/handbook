- Jest - a Test runner
- Enzyme - a react component rendering utility
- Starting Jest 15, we don't need to include Jest's own renderer, just import React, Enzyme and the component you are testing
- Jest will scan and run test file which
  - have `.test.js` suffix in filename
  - have `.spec.js` suffix in filename
  - are in `__tests__` directory with `.js` suffix. files under `__tests__` dir don't need to have .test in the file name
- dependencies required for enzyme </br>
  `enzyme` v3 and adapter `enzyme-adapter-react-16` and `jest-enzyme`
- Configure enzyme to use adapter </br>
  `Enzyme.configure({adapter: new Adapter()})`
- Enzyme `shallow()` don't support `useEffect` and `useLayoutEffect` hooks, also `useCallback` doesn't get memoiszed with `shallow()` render
- Debug log `console.log(wrapper.debug());` print enzyme wrapper
