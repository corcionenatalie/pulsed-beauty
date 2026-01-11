import React, { useState } from "react";
import { BrowserRouter, Routes, Route, NavLink, useLocation } from "react-router-dom";
import { motion, AnimatePresence } from "framer-motion";
import {
  Sparkles,
  Calendar,
  MapPin,
  Phone,
  Mail,
  Instagram,
  Star,
  Check,
  Shield,
  Zap,
  Flower2,
  ChevronDown,
  ArrowRight,
  Heart,
  Image as ImageIcon,
  FileText,
} from "lucide-react";

// ==============================
// PULSED BEAUTY — FULL APP FILE
// ==============================

const BRAND = {
  name: "Pulsed Beauty",
  tagline: "Bright results. Calm energy.",
  subtag:
    "Laser hair removal, body sculpting, teeth whitening, permanent makeup, and facials — personalized, modern, and welcoming.",
  bookingUrl: "https://your-booking-link.com",
  instagramUrl: "https://instagram.com/yourhandle",
  phone: "(000) 000-0000",
  email: "hello@pulsedbeauty.com",
  locationLine: "Your City, State",
  hoursLine: "By appointment • Evenings & weekends available",
  legalName: "Pulsed Beauty",
};

const THEME = {
  primary: "bg-rose-600 text-white hover:bg-rose-700",
  blushText: "text-rose-700",
  blushBorder: "border-rose-200/70",
};

function cn(...c) {
  return c.filter(Boolean).join(" ");
}

function ScrollToTop() {
  const { pathname } = useLocation();
  React.useEffect(() => {
    window.scrollTo({ top: 0 });
  }, [pathname]);
  return null;
}

function Container({ children }) {
  return <div className="mx-auto max-w-6xl px-4">{children}</div>;
}

function Header() {
  return (
    <header className="sticky top-0 z-50 border-b bg-white">
      <Container>
        <div className="flex items-center justify-between py-3">
          <div className="font-semibold">{BRAND.name}</div>
          <nav className="hidden md:flex gap-4 text-sm">
            <NavLink to="/services">Services</NavLink>
            <NavLink to="/pricing">Pricing</NavLink>
            <NavLink to="/about">About</NavLink>
            <NavLink to="/faq">FAQ</NavLink>
            <NavLink to="/contact">Contact</NavLink>
          </nav>
        </div>
      </Container>
    </header>
  );
}

function Footer() {
  return (
    <footer className="border-t bg-white mt-16">
      <Container>
        <div className="py-6 text-xs text-neutral-500 text-center">
          © {new Date().getFullYear()} {BRAND.legalName}
        </div>
      </Container>
    </footer>
  );
}

function Page({ title, children }) {
  React.useEffect(() => {
    document.title = title ? `${title} • ${BRAND.name}` : BRAND.name;
  }, [title]);

  return (
    <div className="min-h-screen bg-neutral-50">
      <Header />
      <main className="py-12">{children}</main>
      <Footer />
    </div>
  );
}

function Home() {
  return (
    <Page>
      <Container>
        <h1 className="text-4xl font-semibold">{BRAND.tagline}</h1>
        <p className="mt-4 text-neutral-600">{BRAND.subtag}</p>
        <a
          href={BRAND.bookingUrl}
          className={cn("inline-block mt-6 px-6 py-3 rounded-full", THEME.primary)}
        >
          Book Now
        </a>
      </Container>
    </Page>
  );
}

function Placeholder({ title }) {
  return (
    <Page title={title}>
      <Container>
        <h2 className="text-3xl font-semibold">{title}</h2>
        <p className="mt-3 text-neutral-600">Content coming soon.</p>
      </Container>
    </Page>
  );
}

export default function PulsedBeautyApp() {
  return (
    <BrowserRouter>
      <ScrollToTop />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/services" element={<Placeholder title="Services" />} />
        <Route path="/pricing" element={<Placeholder title="Pricing" />} />
        <Route path="/about" element={<Placeholder title="About" />} />
        <Route path="/faq" element={<Placeholder title="FAQ" />} />
        <Route path="/contact" element={<Placeholder title="Contact" />} />
      </Routes>
    </BrowserRouter>
  );
}
