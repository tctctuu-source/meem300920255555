// tailwind.config.js
const defaultTheme = require('tailwindcss/defaultTheme')

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './src/**/*.{js,jsx,ts,tsx}',
  ],
  theme: {
    extend: {
      backgroundImage: {
        'paper-texture': "url('https://img-wrapper.vercel.app/image?url=https://www.transparenttextures.com/patterns/paper-fibers.png')",
      },
      colors: {
        'brand': {
          'dark-blue': '#082026',
          'mid-blue': '#134D80',
          'light-blue': '#97DDE8',
          'dark-teal': '#004A4A',
          'coral': '#FF5938',
          'pink': '#FFD1E2',
        },
        'ui': {
          'background': '#F0F5FA', // A very light, cool grey
          'surface': '#FFFFFF',
          'text-primary': '#082026',
          'text-secondary': '#134D80',
          'text-light': '#F0F5FA',
        }
      },
      fontFamily: {
        sans: ['Poppins', ...defaultTheme.fontFamily.sans],
        serif: ['Georgia', 'serif'],
      },
    }
  },
  plugins: [],
};
