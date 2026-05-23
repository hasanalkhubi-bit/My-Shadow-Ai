shadow-ai-web/
│── app/
│   │── page.tsx
│   │── success/page.tsx
│   │── cancel/page.tsx
│   │── api/create-checkout-session/route.ts
│
{pachage.json
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
}
  },
  "dependencies": {
    "next": "latest",
    "react": "latest",
    "react-dom": "latest",
    "stripe": "latest"
  }
}"use client";

import { useState } from "react";

export default function Home() {
  const [loading, setLoading] = useState(false);

  const subscribe = async () => {
    setLoading(true);
    const res = await fetch("/api/create-checkout-session", {
      method: "POST"
    });

    const data = await res.json();
    window.location.href = data.url;
  };

  return (
    <main style={styles.container}>
      <h1 style={styles.title}>Shadow AI</h1>
      <p style={styles.subtitle}>يفهمك قبل أن تطلب</p>

      <div style={styles.card}>
        <h2>Premium</h2>
        <p>احصل على ذكاء تنبؤي كامل + تحليلات + Twin AI</p>

        <button onClick={subscribe} style={styles.button}>
          {loading ? "Loading..." : "اشترك الآن"}
        </button>
      </div>
    </main>
  );
}

const styles: any = {
  container: {
    height: "100vh",
    display: "flex",
    flexDirection: "column",
    justifyContent: "center",
    alignItems: "center",
    background: "#0b0b0f",
    color: "white",
    fontFamily: "sans-serif"
  },
  title: { fontSize: 50, marginBottom: 0 },
  subtitle: { opacity: 0.7, marginBottom: 40 },
  card: {
    padding: 30,
    border: "1px solid #333",
    borderRadius: 20,
    textAlign: "center"
  },
  button: {
    marginTop: 20,
    padding: "12px 20px",
    borderRadius: 10,
    border: "none",
    background: "#ff3b3b",
    color: "white",
    cursor: "pointer"
  }
};import Stripe from "stripe";
import { NextResponse } from "next/server";

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY as string);

export async function POST() {
  const session = await stripe.checkout.sessions.create({
    payment_method_types: ["card"],
    mode: "subscription",
    line_items: [
      {
        price_data: {
          currency: "usd",
          product_data: {
            name: "Shadow AI Premium"
          },
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
  return (
    <div style={{ color: "white", background: "#0b0b0f", height: "100vh", display: "flex", justifyContent: "center", alignItems: "center" }}>
      <h1>تم الاشتراك بنجاح 🎉</h1>
    </div>
  );
}export default function Cancel() {
  return (
    <div style={{ color: "white", background: "#0b0b0f", height: "100vh", display: "flex", justifyContent: "center", alignItems: "center" }}>
      <h1>تم إلغاء الدفع</h1>
    </div>
  );
}{
  "version": 2,
  "builds": [
    { "src": "app/**/*", "use": "@vercel/next" }
  ]
}STRIPE_SECRET_KEY=your_stripe_secret_key_here# My-Shadow-Ai
