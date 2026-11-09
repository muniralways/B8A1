# Type Narrowing: TypeScript কীভাবে টাইপ চিনে নেয় ?

## ভুমিকা

TypeScript-এ একটা ভ্যালুর একাধিক সম্ভাব্য টাইপ থাকতে পারে, যেমন `string | number | boolean`। এটাকে union type বলে। কিন্তু সমস্যা হলো এই অবস্থায় TypeScript জানে না ভ্যালুটা আসলে কোন টাইপের, তাই সরাসরি `trim()` বা `toUpperCase()` চালাতে দেয় না। এই অনিশ্চয়তা দূর করে নির্দিষ্ট টাইপ প্রমাণ করাকেই Type Narrowing বলে।

## মূল আলোচনা

সবচেয়ে সহজ উপায় `typeof`। আমার `processInput` ফাংশনে ইনপুট তিন ধরনের হতে পারে, তাই আগে টাইপ চেক করেছি:

```ts
function processInput(value: string | number | boolean): string | number {
  if (typeof value === "string") {
    return value.trim().toLowerCase();
  }
  if (typeof value === "number") {
    return value * 10;
  }
  return value ? "Active" : "Inactive";
}
```

প্রথম `if`-এর ভেতরে TypeScript নিশ্চিতভাবে জানে `value` একটি string, তাই `trim()` লিখতে কোনো এরর আসে না। মজার ব্যাপার হলো শেষ লাইনে আর `typeof` লিখতে হয়নি, কারণ আগের দুইটা `return` হয়ে যাওয়ার পর শুধু boolean-ই বাকি থাকে। এই ব্যাপারটাকে control flow analysis বলে।

অবজেক্টের ক্ষেত্রে `instanceof` কাজে লাগে, আর `in` অপারেটর দিয়ে চেক করা যায় কোনো নির্দিষ্ট প্রপার্টি অবজেক্টে আছে কি না:

```ts
if (input instanceof Date) {
  return input.toISOString();
}

if ("permissions" in person) {
  return `Admin with ${person.permissions.length} permissions`;
}
```

## সংক্ষেপে

Type narrowing-এর আসল সুবিধা হলো, আমরা `any` ব্যবহার না করেও নিশ্চিন্তে টাইপ-নির্দিষ্ট কাজ করতে পারি। ফলে ভুল টাইপে মেথড কল করলে কম্পাইল করার সময়েই ধরা পড়ে, রানটাইমে ক্র্যাশ করার আগেই।
