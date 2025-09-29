import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import { Calendar, MapPin, Trophy, Users, Newspaper, Image, ArrowRight, Loader2, AlertTriangle } from 'lucide-react';
import { motion } from 'framer-motion';
import { supabase } from '../lib/supabase';
import { HomepageBackground, GalleryItem, NewsItem } from '../types';
import HorizontalGallery from '../components/HorizontalGallery';
import SectionDivider from '../components/SectionDivider';

const Home: React.FC = () => {
  const [background, setBackground] = useState<HomepageBackground | null>(null);
  const [gallery, setGallery] = useState<GalleryItem[]>([]);
  const [news, setNews] = useState<NewsItem[]>([]);
  const [loading, setLoading] = useState({ bg: true, gallery: true, news: true });
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const handleError = (err: any, context: string) => {
      console.error(`Error fetching ${context}:`, err);
      setError("Could not load page content. Please ensure your Supabase project is connected and the credentials are correct.");
    };

    const fetchBackground = async () => {
      const { data, error } = await supabase
        .from('homepage_background')
        .select('*')
        .eq('is_active', true)
        .single();
      if (error && error.code !== 'PGRST116') handleError(error, 'background'); // Ignore no rows found error
      else setBackground(data);
      setLoading(prev => ({ ...prev, bg: false }));
    };

    const fetchGallery = async () => {
      const { data, error } = await supabase
        .from('gallery')
        .select('*')
        .order('created_at', { ascending: false })
        .limit(12);
      if (error) handleError(error, 'gallery');
      else setGallery(data || []);
      setLoading(prev => ({ ...prev, gallery: false }));
    };

    const fetchNews = async () => {
      const { data, error } = await supabase
        .from('news')
        .select('*')
        .eq('status', 'published')
        .order('created_at', { ascending: false })
        .limit(3);
      if (error) handleError(error, 'news');
      else setNews(data || []);
      setLoading(prev => ({ ...prev, news: false }));
    };

    fetchBackground();
    fetchGallery();
    fetchNews();
  }, []);

  const HeroBackground = () => {
    if (loading.bg) {
      return <div className="absolute inset-0 bg-brand-dark-blue flex items-center justify-center"><Loader2 className="w-12 h-12 text-ui-text-light animate-spin" /></div>;
    }
    if (!background || error) {
      return <div className="absolute inset-0 bg-gradient-to-br from-brand-dark-blue via-brand-mid-blue to-brand-dark-teal"></div>;
    }
    if (background.type === 'video') {
      return (
        <video
          className="absolute z-0 w-full h-full object-cover"
          autoPlay
          loop
          muted
          playsInline
          src={background.url}
        />
      );
    }
    return (
      <div
        className="absolute z-0 w-full h-full bg-cover bg-center"
        style={{ backgroundImage: `url(${background.url})` }}
      />
    );
  };

  return (
    <div className="relative">
      {error && (
        <div className="bg-amber-400 text-black p-3 text-center fixed top-16 left-0 right-0 z-[100] flex items-center justify-center gap-2">
          <AlertTriangle size={16} />
          {error}
        </div>
      )}
      {/* Hero Section */}
      <section className="relative min-h-screen flex items-center justify-center overflow-hidden">
        <HeroBackground />
        <div className="absolute inset-0 bg-black/50 z-0"></div>

        <div className="relative z-10 text-center px-4 sm:px-6 lg:px-8 max-w-6xl mx-auto">
          <motion.div
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8, delay: 0.2 }}
            className="mb-12"
          >
            <h2 className="text-ui-text-light text-2xl md:text-3xl font-light mb-4 font-fractual">SSF Muhimmath Daawa Sector</h2>
            <h1 className="text-ui-text-light text-5xl sm:text-7xl md:text-8xl lg:text-9xl font-bold mb-6 tracking-wider font-cooper">
              Meem Fest
            </h1>
            <p className="text-ui-text-light text-lg md:text-xl font-fractual">
              <span className="text-fractual">2025</span> OCTOBER 05,06,07,08 <span className="text-fractual">Muhimmathul Muslimeen Education Centre </span>
            </p>
          </motion.div>

          <motion.div
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8, delay: 0.6 }}
            className="flex flex-col sm:flex-row gap-4 justify-center items-center"
          >
            <Link
              to="/results"
              className="bg-brand-coral text-brand-dark-blue px-8 py-3 rounded-lg font-semibold hover:bg-opacity-90 transition-colors shadow-lg"
            >
              View Results
            </Link>
            <Link
              to="/about"
              className="bg-brand-light-blue/10 backdrop-blur-sm text-brand-light-blue px-8 py-3 rounded-lg font-semibold hover:bg-brand-light-blue/20 transition-colors border border-brand-light-blue/30"
            >
              Learn More
            </Link>
          </motion.div>
        </div>
        <SectionDivider style="wave" color="fill-ui-background" />
      </section>

      {/* Quick Links Section */}
      <section className="relative py-16 bg-transparent">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="text-center mb-12">
            <h2 className="text-3xl font-bold text-ui-text-primary mb-4 font-fractual">Explore SSF Muhimmath Daawa Sector</h2>
            <p className="text-ui-text-secondary max-w-2xl mx-auto">
              Discover the various aspects of our institution through these quick access points
            </p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            {[
              { icon: Trophy, title: 'Results', description: 'Check competition results and winners', link: '/results' },
              { icon: Newspaper, title: 'Latest News', description: 'Stay updated with event announcements', link: '/news' },
              { icon: Image, title: 'Gallery', description: 'View photos from past events', link: '/gallery' },
              { icon: Users, title: 'About Us', description: 'Learn about our mission and history', link: '/about' },
              { icon: Calendar, title: 'Events', description: 'Upcoming programs and schedule', link: '/news' },
              { icon: MapPin, title: 'Contact', description: 'Get in touch with organizers', link: '/contact' }
            ].map((item, index) => (
              <motion.div key={item.title} initial={{ opacity: 0, y: 30 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6, delay: index * 0.1 }}>
                <Link to={item.link} className="block p-6 bg-ui-surface rounded-lg border border-black/5 hover:shadow-lg transition-shadow group">
                  <div className="w-12 h-12 bg-brand-mid-blue rounded-lg flex items-center justify-center mb-4 group-hover:scale-110 transition-transform">
                    <item.icon className="w-6 h-6 text-ui-text-light" />
                  </div>
                  <h3 className="text-xl font-semibold text-ui-text-primary mb-2">{item.title}</h3>
                  <p className="text-ui-text-secondary">{item.description}</p>
                </Link>
              </motion.div>
            ))}
          </div>
        </div>
        <SectionDivider style="angle" color="fill-ui-surface" />
      </section>
      
      {/* Gallery Preview */}
      <section className="relative py-16 bg-transparent overflow-hidden bg-paper-texture bg-repeat">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="text-center mb-12">
            <h2 className="text-3xl font-bold text-ui-text-primary mb-4 font-fractual">Gallery Highlights</h2>
            <p className="text-ui-text-secondary max-w-2xl mx-auto">A glimpse into the vibrant moments of Muhimmath.</p>
          </div>
          {loading.gallery ? (
            <div className="flex justify-center h-[350px] items-center">
              <Loader2 className="w-8 h-8 animate-spin text-brand-mid-blue" />
            </div>
          ) : gallery.length > 0 ? (
            <HorizontalGallery items={gallery} />
          ) : (
            <div className="text-center text-ui-text-secondary py-16">
              <Image className="w-12 h-12 mx-auto text-brand-mid-blue/50 mb-4" />
              <h3 className="text-xl font-semibold">The gallery is currently empty.</h3>
              <p>Check back later for photos from our events.</p>
            </div>
          )}
          <div className="text-center mt-12">
            <Link to="/gallery" className="bg-brand-coral text-brand-dark-blue px-8 py-3 rounded-lg font-semibold hover:bg-opacity-90 transition-colors shadow-lg flex items-center gap-2 justify-center mx-auto w-fit">
              View Full Gallery <ArrowRight size={20} />
            </Link>
          </div>
        </div>
        <SectionDivider style="clouds" color="fill-ui-background" />
      </section>

      {/* News Preview */}
      <section className="relative py-16 bg-transparent">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="text-center mb-12">
            <h2 className="text-3xl font-bold text-ui-text-primary mb-4 font-serif">News & Announcements</h2>
            <p className="text-ui-text-secondary max-w-2xl mx-auto">Stay informed with the latest updates from our team.</p>
          </div>
          {loading.news ? <div className="flex justify-center"><Loader2 className="w-8 h-8 animate-spin text-brand-mid-blue" /></div> :
            <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
              {news.map((item, index) => (
                <motion.div key={item.id} initial={{ opacity: 0, y: 30 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6, delay: index * 0.15 }}>
                  <div className="bg-ui-surface rounded-lg overflow-hidden border border-black/5 h-full flex flex-col">
                    <img src={item.image_url || 'https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://placehold.co/600x400/134D80/FFFFFF?text=News'} alt={item.title} className="w-full h-40 object-cover" />
                    <div className="p-6 flex-grow flex flex-col">
                      <p className="text-sm text-brand-mid-blue font-semibold mb-2">{item.category}</p>
                      <h3 className="text-lg font-bold text-ui-text-primary mb-3 flex-grow">{item.title}</h3>
                      <p className="text-xs text-ui-text-secondary">{new Date(item.created_at).toLocaleDateString()}</p>
                    </div>
                  </div>
                </motion.div>
              ))}
            </div>
          }
          <div className="text-center mt-12">
            <Link to="/news" className="bg-brand-coral text-brand-dark-blue px-8 py-3 rounded-lg font-semibold hover:bg-opacity-90 transition-colors shadow-lg flex items-center gap-2 justify-center mx-auto w-fit">
              View More News <ArrowRight size={20} />
            </Link>
          </div>
        </div>
      </section>
    </div>
  );
};

export default Home;
