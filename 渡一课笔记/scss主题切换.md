```scss
$themes: (
	light: (
		bgColor: #fff,
		textColor: #000,
	),
	dark: (
		bgColor: #000,
		textColor: #fff,
	),
);
$currentTheme: light;

@mixin useTheme() {
	@each $key, $value in $themes {
		$currentTheme: $key !global;
		html[theme='#{$key}'] & {
			@content;
		}
	}
}
@function getTheme($key) {
	$ThemeMap: map-get($themes, $currentTheme);
	@return map-get($ThemeMap, $key);
}
.item {
	width: 100px;
	height: 100px;
	@include useTheme {
		background: getTheme('bgColor');
		color: getTheme('textColor');
	}
} 
```
