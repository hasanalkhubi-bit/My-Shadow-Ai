{
  "name": "shadow-ai-web",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "14.2.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "stripe": "latest"
  }
}shadow-ai-web/
│── app/
│   │── layout.tsx
│   │── page.tsx
│   │── success/page.tsx
│   │── cancel/page.tsx
│   │── api/create-checkout-session/route.ts
│
│── package.json
│── vercel.jsonexport default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html>
      <body style={{ margin: 0, fontFamily: "sans-serif", background: "#0b0b0f", color: "white" }}>
        {children}
      </body>
    </html>
  );
}"use client";

import { useState } from "react";

export default function Home() {
  const [loading, setLoading] = useState(false);

  const subscribe = async () => {
    setLoading(true);
    const res = await fetch("/api/create-checkout-session", { method: "POST" });
    const data = await res.json();
    window.location.href = data.url;
  };

  return (
    <main style={{ textAlign: "center", marginTop: 100 }}>
      <h1>Shadow AI</h1>
      <p>يفهمك قبل أن تطلب</p>

      <button onClick={subscribe}>
        {loading ? "Loading..." : "اشترك"}
      </button>
    </main>
  );
}import Stripe from "stripe";
import { NextResponse } from "next/server";

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY as string);

export async function POST() {
  const session = await stripe.checkout.sessions.create({
    mode: "subscription",
    line_items: [
      {
        price_data: {
          currency: "usd",
          product_data: { name: "Shadow AI Premium" },
          unit_amount: 999
        },
        quantity: 1
      }
    ],
    success_url: "https://your-domain.vercel.app/success",
    cancel_url: "https://your-domain.vercel.app/cancel"
  });

  return NextResponse.json({ url: session.url });
}export default function Success() {
  return <h1>تم الاشتراك 🎉</h1>;
}export default function Cancel() {
  return <h1>تم الإلغاء</h1>;
}{
  "version": 2
}
