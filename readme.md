# styled-queries-ts
This module is responsible for easy **injecting media queries into Components** which are made with **styled-components**

_**Styled-queries-ts** is fully compatibile with **TypeScript**. It is delivered with its own types definitions._

## Instalation

```sh
npm install styled-queries-ts
```

## Simple Usage
Just import the module and use it within styled-component with pre-configured breakpoints.

```
    import respond from 'styled-queries-ts'
    
    const Text = styled.p`
        font-size: 2.2rem;
        ${respond('m', 'font-size: 3.4rem;')
    `
```
## Advanced usage

```
    import respond from 'styled-queries-ts'

    interface IText{
        color?: string;
        margin?: string;
        align?: string;
    }

    export default function Component(){
        return <>
            <Text color='purple' margin='2.2rem auto' align='left'>Text with all props</Text>
            <Text color='yellow'>Text with only color passed. The rest styles fallback to default values.</Text>
        </>
    }

    const Text = styled.p<IText>`
        font-size: 2.2rem;
        text-align: center;
        
        ${(p)=>respond('l',`
            font-size: 6rem;
            color: ${p.color};
            margin: ${p.margin?p.margin:'0'};
        `)
        ${(p)=>respond({min: 1024}, `
            text-align: ${p.align?p.align:'center'};
        `
    `
```

## How does it work?

The module exports function which takes two arguments: **(breakpoint, content)**

#### Breakpoint (with pre-configured values)

To use pre-configured breakpoints pass simply a **string** of value:

| Breakpoint | Value |
| ------ | ------ |
| **'xs'** | **(max-width: 319px)** |
|**'s'** | **(min-width: 539px)**|
| **'m'** | **(min-width: 768px)** |
|**'l'** | **(min-width: 992px)**|
| **'xl'** | **(min-width: 1255px)** |
|**'xxl'** | **(min-width: 1662px)**|
| **'tv'** | **(min-width: 2561px)** |

and use it like:

```
    ${respond('m', 'font-size: 3.4rem;')
```

or pass props (use backticks for second argument - content)
```
    ${(p)=>respond('m', `font-size: ${p.fontSize};`)
```

#### Breakpoint (custom value)

Istead of using pre-configured breakpoints define your own as an **object**:

```
    ${respond({min: 1024}, 'font-size: 2.6rem;')
```
_above will result with (min-width: 1024px)_

or pass props (use backticks for second argument - content):
```
    ${(p)=>respond({max: 996}, `font-size: ${p.fontSize};`)
```
_above will result with (max-width: 996px)_

#### Content

It is simply string (or template string ) with CSS styles


## Invalid values

**_If some of the breakpoint will be invalid, for example "skd" or {maxee: 34} the default breakpoint (min-width: 319px) will be injected_**