# `useLatest`
```javascript
function useLatest<T>(value: T) {
  const ref = React.useRef<T>(value)
  ref.current = value
  
  return ref
}
```