# Access Modifiers:  কী  ? 

## ভূমিকা

OOP-এর একটা বড় সুবিধা হলো ডেটা লুকিয়ে রাখা। TypeScript-এ access modifier দিয়ে আমরা ঠিক করে দিতে পারি কোন প্রপার্টি বা মেথড কোথা থেকে ব্যবহার করা যাবে। প্রথমে আমি ভাবতাম এগুলো শুধু সাজসজ্জার জন্য, কিন্তু পরে বুঝলাম এগুলো আসলে ভুল ব্যবহার ঠেকিয়ে দেয়।

## মূল আলোচনা

ডিফল্টভাবে সব প্রপার্টি `public`, মানে যেকোনো জায়গা থেকে ব্যবহার করা যায়। `private` দিলে শুধু নিজের ক্লাসের ভেতরে ব্যবহার করা যায়, সাবক্লাসেও না। আর `protected` দিলে নিজের ক্লাস এবং সাবক্লাসে ব্যবহার করা যায়, কিন্তু বাইরে থেকে না।

আমার `Employee` আর `Manager` ক্লাসে `name`-কে `protected` রেখেছি, কারণ `Manager`-এর `getRoleInfo()` মেথডে `this.name` দরকার। কিন্তু `baseSalary` `private`, কারণ সাবক্লাসের এটা দরকার নেই।

```ts
class Employee {
  protected name: string;
  private baseSalary: number;

  constructor(name: string, baseSalary: number) {
    this.name = name;
    this.baseSalary = baseSalary;
  }
}

const e = new Employee("Alice", 50000);
e.name;        // Error: protected
e.baseSalary;  // Error: private
```

`readonly` আরেকটু আলাদা। এটি অ্যাক্সেস নিয়ন্ত্রণ করে না, বরং মান একবার সেট হলে আর বদলানো যাবে না। এটা constructor-এর ভেতরে সেট করা যায়, বাইরে থেকে পরে নয়।

একটা কথা মনে রাখা ভালো, এগুলো সবই কম্পাইল-টাইম চেক। কম্পাইল হওয়া JavaScript-এ `private` বলে কিছু থাকে না, তাই রানটাইমে কেউ চাইলে ভেতরে ঢুকে পড়তে পারে।

## Conclusion
 মুল কথা হলো
`public` সবার জন্য, `private` শুধু নিজের জন্য, `protected` নিজে ও সাব ক্লাসের জন্য, আর `readonly` মান অপরিবর্তনীয় রাখার জন্য। ঠিকভাবে ব্যবহার করলে কোড অনেক নিরাপদ আর পড়তে সহজ হয়।
